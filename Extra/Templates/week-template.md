---
tags:
  - weekly
<%*
// Parse current note date from filename (assumes "gggg-[W]ww")
// `gggg` is "Locale 4 digit week year".
// See https://momentjs.com/docs/#/parsing/string-format/
let fileTitle = tp.file.title;
let parts = fileTitle.split('-W');
if (parts.length < 2) {
  tR += "Invalid format";
} else {
  let year = parseInt(parts[0]);
  let week = parseInt(parts[1]);
  let startOfWeek = moment().weekYear(year).week(week).startOf("week");

  // Next and prev week
  tR += "prev: \"[[" + moment(startOfWeek).subtract(7, "days").format("gggg-[W]ww") + "]]\"\n";
  tR += "next: \"[[" + moment(startOfWeek).add(7, "days").format("gggg-[W]ww") + "]]\"\n";

  // Close properties
  tR += "---\n"

  // Generate a link for each day from Monday to Friday
  for (let i = 0; i < 5; i++) {
    let day = moment(startOfWeek).add(i, "days").format("YYYY-MM-DD");
    tR += `- [[${day}]]\n`;
  }
}
%>
---

## Todo

## Projects

## Notes
