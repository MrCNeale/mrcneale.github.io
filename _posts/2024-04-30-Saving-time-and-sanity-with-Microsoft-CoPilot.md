---
layout: post
title: How to Use Microsoft CoPilot to do repetitive/simple tasks or turn illegible code into English
excerpt: Using CoPilot to understand quite bad legacy code and refactor it 
comments: true
date: 2024-04-30T09:00:00+00:00
image:
  feature: copilot.jpg
tags: 
  - CoPilot
  - AI
  - Code
  - Refactoring
  - Time saving
---

<img src="/public/copilot.jpg" alt="copilot" width="100" height="100"/>

# They came, they coded, they left...and now the fix has fallen in your lap!
Recently I had a bug come in about a section of code that was written some time ago for a small custom project.  The excerpt isn't private IP (to be honest I would advise against using it!  Code getting adopted by other teams and then being made the domain of others to maintain is not uncommon).  
It was part of some prioprietary code to start and stop VMs.  Sounds simple, right?  Well let's just say that the person who wrote it didn't feel the need to optimise it or comment it to explain their unique way of determining cases.

Here's the code sample that we had to work with...

```Powershell
if (($vmScheduleArray[4] -eq 'untilNextDay' -or ((Convert-TimeZone $setTimeZone "-Format ddd") -match $vmScheduledWeekDay.Replace(',', '|'))) `
        -and ((((((Convert-TimeZone $setTimeZone DayOfWeek) -match $vmScheduledWeekDay.Replace(',', '|') -or ((Convert-TimeZone $setTimeZone "-Format ddd") -match $vmScheduledWeekDay.Replace(',', '|'))) -and ((Convert-TimeZone $setTimeZone "-Format dd") -match $vmScheduledDayOfMonth.Replace(',', '|'))) `
                    -and ((($vmScheduleArray[0] -eq "allDay" <#-or $vmScheduleArray[4] -match '\d\w' #>) -and [int](Convert-TimeZone $setTimeZone).TimeOfDay.TotalMinutes -in $vmShutdownStart..$vmDowntimeEnd))) `
                -or ((((Convert-TimeZone $setTimeZone "-Format ddd") -match $vmScheduledWeekDay.Replace(',', '|')) -and $vmScheduleArray[4] -eq 'untilNextDay' -and ((Convert-TimeZone $setTimeZone "-Format dd") -match $vmScheduledDayOfMonth.Replace(',', '|'))) `
                    -and ([int](Convert-TimeZone $setTimeZone).TimeOfDay.TotalMinutes -in $vmShutdownStart..$vmShutdownEndAnyDay)) `
                -or ((((Convert-TimeZone $setTimeZone DayOfWeek) -match $vmScheduledWeekDay.Replace(',', '|') -and ((Convert-TimeZone $setTimeZone "-Format dd") -match $vmScheduledDayOfMonth.Replace(',', '|')))`
                        -and ([int](Convert-TimeZone $setTimeZone).TimeOfDay.TotalMinutes -in $vmShutdownStart..$vmShutdownEnd)) -and ((Convert-TimeZone $setTimeZone "-Format dd") -match $vmScheduledDayOfMonth.Replace(',', '|'))))) `
        -and ((Convert-TimeZone $setTimeZone "-Format MMM") -match $vmScheduledMonth.Replace(',', '|'))) {
    return "Stop"
    break

}
```

Without knowing much about what it was trying to do, but knowing that the issue lay in the code starting/stopping VMs when it wasn't supposed to, I wondered where to start.  But I was trying to help others so I thought I'd get CoPilot to first describe what it's doing in English.
This went pretty well:  
```
Sure, here’s a simplified English interpretation of your if statement:
If the VM is scheduled to run until the next day OR the current day of the week matches the VM’s scheduled day of the week.
AND
The current day of the month matches the VM’s scheduled day of the month AND one of the following conditions is true:
The VM is scheduled to run all day AND the current time is within the VM’s scheduled downtime.
OR
The current day of the week matches the VM’s scheduled day of the week AND the VM is scheduled to run until the next day AND the current time is within the VM’s scheduled shutdown time for any day.
OR
The current day of the week matches the VM’s scheduled day of the week AND the current time is within the VM’s scheduled shutdown time AND the current day of the month matches the VM’s scheduled day of the month.
AND
The current month matches the VM’s scheduled month.
This interpretation removes the time zone conversion functions and simplifies the conditions. However, please note that this is a high-level interpretation and the exact conditions may vary based on the specific values and functions used in your script. If you need further clarification, feel free to ask!
```

It explains it in plain English and gives the caveat that it's removed some bits to improve readability but use it with caution as it may not be perfect.  Fair warning!

So going back to the code itself, surely we can make the code read more like the English to make it easier for future coders to understand and fix/alert it.
Straight away it seems odd that there is so much repetition.  There are values and functions, such as the time-zone conversion and existing values for schedule week day or month etc.  Those also have in-line replacements repeated over and over.
I asked Co-Pilot again to "Simplify the code and replace repeated statements with suitably named variables".
This quickly got us to this
```Powershell
$dayOfWeekMatch = $setTimeZone.DayOfWeek -match $vmScheduledWeekDay.Replace(',','|')
$dayOfMonthMatch = $setTimeZone.Day -match $vmScheduledDayOfMonth.Replace(',','|')
$monthMatch = $setTimeZone.Month -match $vmScheduledMonth.Replace(',','|')
$timeOfDayMinutes = [int]$setTimeZone.TimeOfDay.TotalMinutes
 
if (($vmScheduleArray[4] -eq 'untilNextDay' -or $dayOfWeekMatch) `
    -and $dayOfMonthMatch
    -and $monthMatch
    -and (
        (($vmScheduleArray[0] -eq "allDay") -and $timeOfDayMinutes -in $vmShutdownStart..$vmDowntimeEnd)
        -or ($timeOfDayMinutes -in $vmShutdownStart..$vmShutdownEnd)
    )
)
```

This would be great, but...being AI/computer it just did what I asked.  Hopefully most of you are thinking "let's just replace that finally duplication with a more descriptive variable...say "InsideShutdownWindow".
So the final revision that we first re-tested, then started to debug and fix was this:

```Powershell
$dayOfWeekMatch = $setTimeZone.DayOfWeek -match $vmScheduledWeekDay.Replace(',','|')
$dayOfMonthMatch = $setTimeZone.Day -match $vmScheduledDayOfMonth.Replace(',','|')
$monthMatch = $setTimeZone.Month -match $vmScheduledMonth.Replace(',','|')
$timeOfDayMinutes = [int]$setTimeZone.TimeOfDay.TotalMinutes
$insideVmShutdownWindowMatch = $timeOfDayMinutes -in $vmShutdownStart..$vmShutdownEnd
 
if (($vmScheduleArray[4] -eq 'untilNextDay' -or $dayOfWeekMatch) `
    -and $dayOfMonthMatch
    -and $monthMatch
    -and (
        (($vmScheduleArray[0] -eq "allDay") -and $insideVmShutdownWindowMatch)
        -or $insideVmShutdownWindowMatch
    )
)
```

Not rocket science, and something a junior programmer could do.  But at first glance the confusing nature of the giant IF statement makes you feel like you're in for a long afternoon of manual refactoring...and it would help if you understood what it was doing first.  CoPilot did give a good explanation of that, and it refactored the code in seconds/minutes versus a fair bit longer for a human and much more likelihood of typos or missing/messing up brackets.

This is the first big use case for AI to me.    
Do the repetitive stuff, do it quickly.  
Scan uncommented/documented code and explain it to me, and do it quickly.  
Of course you could use a.n.other AI of your choice, others are available!
