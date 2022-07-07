---
layout: page
title: Intro on PDF generator
categories: [pdf node]
tags: [nest.js node handlebar puppeteer]
---

# Welcome to my advanced Node journey

In my last job, I accidentally was assigned to created a PDF generator using a modern tech stack :
- Node
- Nest.js
- Handlebar
- i18n
- Puppeteer

---
## **Requirements**

The PDF generator has the following requirements:
1. Stateless, meaning it will only take in input as HTTP Post Body in json form. Then output a rendered PDF.
2. Multi-Lingual, needs to support multiple language simultaneously.
3. Multi-Timezones, needs to support whichever browser's timezone requesting the PDF and display all time to local time.
4. Multi-Currencies, needs to display in different currencies simultaneously.
5. Performant, needs to render a complex PDF within 2 seconds.
6. Scalable, needs to be able to spawn hundred or thousands of PDF generator to service heavy traffic demand.

---
## **Disclaimer**
I architected and implement a solution that met all requirements. It was such a success it was deployed through-out  a larger international brand company in North America and Europe in less than 10 months.

After I left the company, I re-implemented a brand new system from scratch in clean room fashion. I took the conceptual learnings and implemented an even more superior system. Through out this blob, I will show fragments of new clean code without any copyright violation.

The code I created is NOT open source, but I want to share my experience from a higher level perspective.