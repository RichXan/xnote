---
tag:
  - Daily
week: <% moment(tp.file.title, "YYYY-MM-DD").format("gggg-[W]WW") %>
weekday: <% moment(tp.file.title, "YYYY-MM-DD").format("dddd") %>
aliases:
location:
mood:
sleep:
weight:
meditation:
exercise:
study:
---
<%*
let titleDate = moment(tp.file.title);

// # 2023/01/01 - Sunday
tR += '# ' + titleDate.format('dddd, DD MMMM YYYY') + '\n';

// 2023 / Q1 / January / Week 1
tR += '[[' + titleDate.format('YYYY') + ']] / ';
tR += '[[' + titleDate.format('YYYY-[Q]Q') + '|' + titleDate.format('[Q]Q') + ']] / ';
tR += '[[' + titleDate.format('YYYY-MM') + '|' + titleDate.format('MMMM') + ']] / ';
tR += '[[' + titleDate.format('gggg-[W]WW') + '|' + titleDate.format('[Week] w') + ']]';
tR += '\n\n';

// ❮ 2022-12-31 | 2023-01-01 | 2023-01-02 ❯
tR += '❮ [[' + titleDate.subtract(1, 'days').format('YYYY-MM-DD') + ']]';
tR += ' | ' + titleDate.add(1, 'days').format('YYYY-MM-DD') + ' | ';
tR += '[[' + titleDate.add(1, 'days').format('YYYY-MM-DD') + ']] ❯';
titleDate.subtract(1, 'days');
%>

```dataview
table without id
	mood AS "🌄",
	choice(meditation,"✅","❌") AS "🧘‍♂️",
	choice(exercise,"✅","❌") AS "💪",
	choice(study,"✅","❌") AS "📚"
from "WorkLog/Paramita/Daily"
where file.name = "<% moment(tp.file.title).format('YYYY-MM-DD') %>"
```
```dataviewjs
// List of gratitudes
let gratitudes = [];

// Extract gratitudes from pages that have them
dv.pages()
	.where(page => page.gratitude)
	.forEach(page => {
		dv.array(page.gratitude)
			.forEach(gratitude => {
				gratitudes.push({
					message: gratitude,
					page: page
				});
				})});

// List of awesome things
let learnings = [];
// Extract learnings from pages that have them
dv.pages()
	.where(page => page.learning)
	.forEach(page => {
		dv.array(page.learning)
			.forEach(learning => {
				learnings.push({
					message: learning,
					page: page
				});
				})});

let gratitudegreeting = gratitudes[Math.floor(Math.random()*gratitudes.length)] 
let learninggreeting = learnings[Math.floor(Math.random()*learnings.length)]

let text = "";
if (gratitudegreeting) text += `*Practice gratitude:* ${gratitudegreeting.message} (${gratitudegreeting.page.file.link})<br>`;
if (learninggreeting) text += `*A random learning:* ${learninggreeting.message} (${learninggreeting.page.file.link})`;
if (!text) text = "No gratitude or learning entries found yet.";

dv.paragraph(text);
```
<% tp.web.daily_quote() %>

![[WorkLog/Paramita/Weekly/<%moment(tp.file.title).format("gggg-[W]WW")%>#Goals for this week:]]

## Today's Tasks
 - [ ] 30 new words
 - [ ] 
 - [ ] 

## What am I grateful for? 
> Things you are grateful for, things you feel happy about
-  Gratitude:: 

## Unhappy of the day?
> What makes you unhappy today
- 

## What is your inner thoughts today? 
> Today's inner thoughts
- Learning:: 

> [!note]- Files created on this day
>```dataview  
>LIST WHERE file.cday = date(this.file.name)
>```

