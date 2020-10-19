---
layout: post
Title: Don't reference methods in a template
Tags: angular templates performance change-detection
---

 layout:

- explain where you have experienced this problem
- show an example of the problem and why it seemed like a good solution
- Explain why this is the incorrect solution
- Discuss available options and what is best in this case
- List other situations similar to this one where this incorrect solution could be used, and list what shoud be done instead.



I was recently asked to look at the performance of one of our products. It is a planning tool - it's main feature is a large planning board filled with task cards, that can be dragged around, rearranged and assigned to various members of the team to work on.

When the project was at the spectfication phase, the product owners said that their clients already had the task-cards made and new exactly how many cards there would be 628. Of course they might ass a few here and there to clarify things, but a working number of 1200 cards per project would give plenty of room for the product to grow.

I have no doubt you knwo what is coming next.

5000 was about the average as I looked into the performance - and the performance was bad, I mean really bad. The loading times where outstanding - but in a bad way. 17s load time for a 5000 card project. The drag and drop was more of a drag-and-move-the-mouse, on drop, if you were lucky, the card would end up in the right place and you could try another. 
this kind of performance does not give the user a warm-and-fuzzy feeling, it makes them want to rage-quit their job - and that is _my_ _fault_.

Queue tiny violins

After some digging into the code I noticed something interesting. Every card a project had different color combinations to show their status: red is late, yellow is on time, green is completed (in reality it was a ot more complex than this) and there was a small method in the template that returned a string of styles for the task card in question.

This small method was not big, or very complex but I placed a console.log in there to check it was being called as expected. Sure enought the method did run. a _lot_.

By a lot I mean almost constantly, erlly there was no way of using the log when this console.log was there, as any other value would be swept off the top of the screen before it had fully rendered. As console.logs are quite expensive performance wise too, the app ground to a halt, the fan speed was reminiscent of a catherine wheel and you could have cooked a small family meal on the laptop - let alone fry an egg.

In fear my laptop was going to cause a housefire I killed the crome process, removed the console log and wondered what the fuck just happened. 

After running my laptop unter a cold tap for 15-20minutes it was a lot cooler and so I started to consider the classical philosophy of 'what the fuck just happened'.

Well, I knew the method was not too complex, but it was being called a lot, everytime you moved the mouse: must be events. then why was it running when it was completely unrelated to the drag and drop? Change Detection.

Each time the (unforunate) user moved the mouse, the mousein/mouseout/mousemove events began firing - fast. Each of these events caused a change detection cycle on the top-level component (the planner). This CD cycle then propegated down to its child components too: the task-cards. 
Thats 5000 

#Don't reference methods in a template 

When we are given so many tools by Angular - it can be hard to know exactly how to use them all effectively. 



I often see this pattern in merge requests. What the programmer is trying to do is dynamicaly add styles to an element based on that elements properties. You can understand why - this is exactly what we're using these frameworks for right - to make this kind of thing easy.

Well, yes and no.

The yes: Yes the framework authors where hoping to make styling elements based on some peice of data more simple and efficient.

The no: This is not how they intended you to do it!

Why?

Each time change detection runs, each method in your template is going to be called. Every single one, every single time. 



well, this is all about change detection really. If your unsure what change detection is or when it fires then please read my other article linked here.



You may argue that in your case this is not be much of an issue - maybe you have really carefully managed CD cycles, maybe the function is so well written that it really wont matter if it called each time? but with both of these comes other issues, what happens when in 6months time you forget about your manicured CD cycles, or another develper removes the momoization from your hyper-efficient function because it 'wastes memory'.



Do not put method calls in templates unless you are very sure you know what you are doing, and have mitigated for the next programmer who might not!

```html
/progress-bar/progress-bar.component.html
<div class="progress-bar">
  <ng-container *ngFor="let bar of barList">
    <div class="bar" [ngStyle]=barStyle(bar)></div>
  </ng-container>
</div>
```

```typescript
/progress-bar/progress-bar.component.ts

const COLOR_UNDER_THRESHOLD = '#FFDD00';
const COLOR_OVER_TRESHOLD = '#3EFE9E';

export class ProgressBarComponent implements OnInit {

  @Input() barList: Bar[];
  @Input() threshold?: number;

  constructor() { }

  get barStyle(bar) {
    return {
      background: `${bar.matchingLevel < this.threshold ? COLOR_UNDER_THRESHOLD : COLOR_OVER_TRESHOLD}`,
      width: `${bar.matchingLevel}%`
    }

```



The reason is that the barStyle method will be called on every change detection cycle, which can be a LOT!
This method is currently quite small, but if has a more complex calculation it could affect performance.
There are 2 ways to mitigate this  

1. Don't add method call at all. Use classes and ngClass to add and remove styles.
2. Change the change detection method in this component so the number of Change Detection cycles is greatly reduced.
3. 

