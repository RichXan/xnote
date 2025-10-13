---
week: <% moment(tp.file.title, "YYYY-MM-DD").format("gggg-[W]WW") %>
weekday: <% moment(tp.file.title, "YYYY-MM-DD").format("dddd") %>
aliases:
location:
mood:
tag:
  - Daily
sleep:
weight:
prayer:
meditation:
exercise:
study:
stretch:
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
	mood + " #_/habits" AS "🌄",
	prayer AS "🙏",
	choice(meditation,"✅","❌") AS "🧘‍♂️",
	choice(exercise,"✅","❌") AS "🏃‍♂️",
	choice(study,"✅","❌") AS "📚",
	choice(stretch,"✅","❌") AS "🤸"
from "Journal/Daily"
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

## What am I grateful for?
1. Gratitude:: 

> [!todo]- Tasks of the day
>```todoist  
>name: '' 
>filter: "(!#Work) & due before: tomorrow"
>sorting:  
>- date  
>- priority  
>group: true  
>```

## Highlights of the day:
1. Highlight:: 

## What did I learn today?
1. Learning:: 

## What good have I done today?
1. Good:: 

## What excited or drained me?
1. Exciting::
2. Draining:: 
```dataviewjs
// List of kindle highlights
let kindles = [];

// Extract kindles from pages that have them
dv.pages()
	.where(page => page.kindle)
	.forEach(page => {
		dv.array(page.kindle)
			.forEach(kindle => {
				kindles.push({
					message: kindle,
					page: page
				});
				})});

// 如果没有任何 kindle highlight，给出默认提示
if (kindles.length === 0) {
	dv.paragraph("*Kindle highlight:* No highlights found.");
} else {
	let kindlehighlight = kindles[Math.floor(Math.random() * kindles.length)];
	dv.paragraph("*Kindle highlight:* " + kindlehighlight.message + " (" + kindlehighlight.page.file.link + ")");
}

dv.paragraph("*Kindle highlight:* " + kindlehighlight.message + " (" + kindlehighlight.page.file.link + ")");
```
```dataviewjs
// List of podcast highlights
let podcasts = [];

// Extract kindles from pages that have them
dv.pages()
	.where(page => page.podcast)
	.forEach(page => {
		dv.array(page.podcast)
			.forEach(podcast => {
				podcasts.push({
					message: podcast,
					page: page
				});
				})});


let podcasthighlight = podcasts[Math.floor(Math.random()*podcasts.length)] 

dv.paragraph("*Podcast highlight:* " + podcasthighlight.message + " (" + podcasthighlight.page.file.link + ")");
```
#### Random notes
```dataviewjs
const noOfNotes = 3

app.vault.getFiles()
const files = app.vault.getFiles().filter(f => !f.path.includes("Journal") && !f.path.includes("Templates") && !f.path.includes("textgenerator") && !f.path.includes("Assets"))
const random = Math.floor(Math.random() * (files.length - 1))
const randomNote = files[random]

for (let i = 0; i < noOfNotes; i++) {
  const random = Math.floor(Math.random() * 
                            (files.length - 1))
  const randomNote = files[random] 
  dv.paragraph('[[' + randomNote.basename + ']]')
  }
```

> [!note]- Files created on this day
>```dataview  
>LIST WHERE file.cday = date(this.file.name)
>```

