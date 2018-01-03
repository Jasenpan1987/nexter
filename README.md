# CSS grid
## Properties
### 1. Container properties:
1.1 template

grid-template-rows

grid-template-columns

grid-template-area

1.2 gap

grid-row-gap

grid-column-gap

justify-items

align-items

justify-content

align-content

grid-auto-rows

grid-auto-columns

grid-auto-flow

### 2. Item properties:
grid-row

grid-row-start

grid-row-end

grid-column

grid-column-start

grid-column-end

grid-area

### 3. Self properties
justify-self

align-self

Demo 1: 
https://codepen.io/anon/pen/vpmxMm?editors=1100

## Notes
1. `grid-template-row: repeat(2, 150px)`:
equals to `grid-template-row: 150px 150px`

2. `1fr`: 1fraction unit, which means to occupies all the remaining space of a track, for instance, `grid-template-columns: repeat(3, 1fr)`. fr is a basically a unit for ratios. And also, it can be mixed with other unit such as `grid-template-columns: 50% 1fr 1fr`; means the first column takes half of the spaces and the 2nd and 3rd one takes 1 quarter each, this kind of mixture will not take the gap into account.

3. Currently, firefox has grid friendly devtool, we can use that to help us build the grid layout. Devtool -> layout -> overlay grid -> display line numbers

### Shor hand
```
.item1 {
  background-color: orange;
  grid-row: 2/3;
  grid-column: 3/4;
}
```

## Expand Cells
Expand cell items:
Simply give the row or column a higher number so it can grow. But it will push the other items down or right. And I can set the items visible by setting the z-index to a higher value.
  .item3 {
    background-color: palevioletred;
    grid-row: 2 / 3;
    grid-column: 1 / 3;
  }

Alternatively, we can set the span property with a value
```
.item2 {
  background-color: green;
  grid-column: 1 / span 2; // here it means the item start with column 1 and will span 2 columns
}
```
If I want to span an item to the end, I can put -1,
```
.item3 {
  background-color: palevioletred;
  grid-row: 2 / -1; // starts from the 2nd row, end at the end
  grid-column: 1 / 3;
}
```
Example: create a small website layout:
https://codepen.io/anon/pen/dJWWxK?editors=1100

## Naming grid lines:
Instead of the 1, 2, 3 … we can give the name for the grid lines.
```
grid-template-rows:
	[header-start] 50px [header-end box-start] 100px 
	[box-end main-start] 200px [main-end footer-start] 50px [footer-end];

  grid-template-columns: repeat(3, [col-start] 1fr [col-end]) 100px[grid-end]; 
```
For repeating, the col-start and col-end will be transformed to col-start 1, col-start 2... col-end 1, col-end 2...

To use it, we can simply reference to the line names instead of the line numbers, and this is a more professional practice.

Example: 
https://codepen.io/anon/pen/OzmjQj?editors=1100

## Naming grid areas:
(This is very useful for small layouts, and we must fill in each cell, otherwise it will not work.)

We can also give each cell a name by doing this after we defines the grid-template-rows and grid-template-columns
```
grid-template-areas: 
    "head head head head"
    "box1  .  box2  side"
    "main main main side"
    "foot foot foot side";
```
Each of these rows and columns corresponding to a cell or column in the actual grid layout, and the . means an empty cell.

Then when we set the cell positions, we can do something like this: grid-area: head;



Example:
https://codepen.io/anon/pen/vpmeYZ?editors=1100

This technique can be useful for smaller layouts but not very useful for the big ones.


## Style the explicit cells
In grid system, the items with explicit row and column tracks are call explicit cells, it is kind of “undefined” by the grid layout. For example, in a grid layout, if we only define a 2X2 grid, and at the same time, we have 8 items, the last 4 will become explicit items.

We can define the style for all of these explicit cells by using
```
grid-auto-rows: 50px; 
grid-auto-columns: .5fr;
```

And we can set the flow direction for these cells by using 
```
grid-auto-flow: column;
grid-auto-flow: row dense;
```
The dense will make it automatically fill the empty cells.

Example:
https://codepen.io/anon/pen/mpmBEp?editors=1100

## Align item in the cell
In the previous examples, the actual content within a cell are all expand to fill the entire cell (both horizontally and vertically), but it is not always what we want.

In grid layout, we have properties just work similarly like in flex box, the align and justify.

In the container element, we can set align-items to center, start and end, which position the content in the specific location in Y axis. The default is stretch.

In the container element, we can set justify-items to center, start and end, which position the content in the specific location in X axis. The default is stretch.

These will define all the content in cells. But if we want to overwrite the alignment defined in the container for a specific cell item, we can use `align-self` or `justify-self`.
`align-self: stretch;`
`justify-self: stretch;`
The available values will be the same as the ones in the container.

Example:
https://codepen.io/anon/pen/jYmGQO?editors=1100

## Align the entire grid content
Just like flex-box, we can align the entire grid content (all the cells as a whole) vertically and horizontally.

To align the content horizontally, we can use 
`justify-content: center;`

To align the content vertically, we can use
`align-content: center;`

The available options for these two properties are stretch (default), center, start, end, space-between, space-around and space-evenly. These options work exactly the same as in flex-box.

## Minmax function
min-content: take content space as less as possible, the content can be wrapped.

max-content: take as content space as much as possible, the content will not be wrapped.

minmax: a function that gives the min and max value to a row or column.

Example:
https://codepen.io/anon/pen/BJRmJN?editors=1100

## Adapted responsive layout:
We can use the following trick to build the responsive grid layout without media query

1. Set a container a percentage width: width: 80%;
2. Give the column a fraction fit width, and repeat it with auto fit times: grid-template-columns: repeat(auto-fit, minmax(100px, 1fr)); 
The grid will shrink until it reaches the 100px limit, then it will go to the next line.

Steps of building grid columns in real life project
1. Normally, we start with a pre-defined grid layout, such as 8 column layout or 12 column layout.
grid-template-columns: repeat(8, 1fr);

2. Then we make the width of each column to a static number
grid-template-columns: repeat(8, 14rem);

3. We can then make the columns responsive by using the mixmax functions, here it basically means the max width is 14rem, and it will be at least wrap the content with line breaks
grid-template-columns: repeat(8, minmax(min-content, 14rem));

### 
How to make an image 100% fit in a container
1. Create a wrapper for the image
2. Using the following trick
```
&__img {
  width: 100%;
  height: 100%;
  display: block;
  object-fit: cover;
}
```
