# Excel 到提醒事项与日历同步工作流

## 目录

- [Excel 解析](#excel-解析)
- [提醒事项同步](#提醒事项同步)
- [日历同步](#日历同步)
- [验证与修正](#验证与修正)
- [常见问题](#常见问题)

## Excel 解析

优先使用当前运行环境已有的 Python 与 `openpyxl` 读取 `.xlsx`：

```python
import openpyxl

wb = openpyxl.load_workbook(path, data_only=True)
ws = wb[wb.sheetnames[0]]
headers = [cell.value for cell in ws[1]]
```

层级列经常因为视觉合并单元格而在后续行为空。解析时对这些列向下补齐：

```python
carry = {h: None for h in ["系统", "一级分类", "二级分类", "三级分类"]}
for row in ws.iter_rows(min_row=2, values_only=True):
    item = dict(zip(headers, row))
    for key in carry:
        if item.get(key) not in (None, ""):
            carry[key] = item[key]
        else:
            item[key] = carry[key]
```

标题中的分类路径建议由这些字段组成：

```text
需求部门 / 一级分类 / 二级分类 / 三级分类 / 功能点
```

跳过空字段，避免出现连续分隔符。

## 提醒事项同步

默认写入 `开发项` 列表。AppleScript 逻辑：

```applescript
tell application "Reminders"
  set targetListName to "开发项"
  if not (exists list targetListName) then make new list with properties {name:targetListName}
  set targetList to list targetListName

  set reminderTitle to "[17] 待开发 - 云实 / 产品管理 / 产品列表 / 新增产品+"
  if exists (first reminder of targetList whose name is reminderTitle) then
    -- skip
  else
    make new reminder at end of reminders of targetList with properties {name:reminderTitle, body:"..."}
  end if
end tell
```

重复判断使用完整标题。不要只用功能点判断，因为不同分类下可能都有“操作-编辑”“操作-详情”。

## 日历同步

默认写入 `开发项` 日历。日期字段映射：

```text
完成规划时间 -> 完成规划
开发完成时间 -> 开发完成
计划上线时间 -> 计划上线
```

创建或更新日程时用完整标题做唯一键：

```applescript
tell application "Calendar"
  set calName to "开发项"
  if not (exists calendar calName) then make new calendar with properties {name:calName}
  set targetCal to calendar calName

  set matchedEvents to (every event of targetCal whose summary is eventTitle)
  if (count of matchedEvents) > 0 then
    set ev to item 1 of matchedEvents
    set start date of ev to startDate
    set end date of ev to endDate
    set allday event of ev to true
    set description of ev to eventNotes
  else
    make new event at end of events of targetCal with properties {summary:eventTitle, start date:startDate, end date:endDate, allday event:true, description:eventNotes}
  end if
end tell
```

全天事件的结束时间必须保持在同一天：

```applescript
set startDate to current date
set year of startDate to 2026
set month of startDate to 5
set day of startDate to 18
set time of startDate to 0
set endDate to startDate + (1 * days) - 1
```

不要使用 `startDate + (1 * days)` 作为全天事件结束时间，否则用户界面可能显示多一天。

## 验证与修正

提醒事项数量验证：

```applescript
tell application "Reminders" to count (reminders of list "开发项" whose name contains "待开发 -")
```

日历数量验证：

```applescript
tell application "Calendar" to count (events of calendar "开发项" whose summary contains "[17]")
```

如需修正已经跨天的全天事件：

```applescript
tell application "Calendar"
  set targetCal to calendar "开发项"
  repeat with ev in (events of targetCal whose summary contains "[17]")
    set sd to start date of ev
    set time of sd to 0
    set end date of ev to sd + (1 * days) - 1
    set allday event of ev to true
  end repeat
end tell
```

## 常见问题

如果 Calendar 或 Reminders 权限被拒绝，说明需要用户授权 Automation/Calendars/Reminders 权限。不要绕过系统权限。

如果工作表有多个 sheet，先列出 sheet 名和包含目标字段的 sheet，再选择最可能的项目总表。若无法判断，询问用户。

如果日期是文本，先尝试解析 `YYYY-MM-DD`、`YYYY/MM/DD` 或 Excel 已识别的 date/datetime。解析失败时跳过该日期并在最终说明中列出。

如果用户只要求提醒事项，不要顺手写日历；如果只要求日历，不要顺手写提醒事项。
