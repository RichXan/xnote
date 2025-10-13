---
tags:
  - daily
<%*
// Parse current note date from filename (assumes "YYYY-MM-DD")
let d = moment(tp.file.title, "YYYY-MM-DD");
if (!d.isValid()){
    tR += "Invalid date format";
} else {
    tR += "week: \"[[" + d.format("gggg-[W]ww") + "]]\"\n";

    let prev, next;
    if (d.isoWeekday() === 1) {
        prev = moment(d).subtract(3, "days"); // Skip weekend
    } else {
        prev = moment(d).subtract(1, "days");
    }
    if (d.isoWeekday() === 5) {
        next = moment(d).add(3, "days"); // Skip weekend
    } else {
        next = moment(d).add(1, "days");
    }
    tR += "prev: \"[[" + prev.format("YYYY-MM-DD") + "]]\"\n";
    tR += "next: \"[[" + next.format("YYYY-MM-DD") + "]]\"\n";
}
%>
---
## Meetings
- 

## Todo
- [ ] 

## Notes
