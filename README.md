# react-projects-menu
* Functionality that focuses on this project 
1. Buttons that allow us to filter the list.

2. These buttons will be added dynamically.
	
3. Using the Set data structure

### Button manual approach
Have buttons for each category and buttons for all.
And once we click on a button, we display only the items that match that category.

>App.js file
```js
function App() {
 const [menuItems, setMenuItems] = useState(items);
 const [categories, setCategories] = useState([]);
 
 const filterItems = (category) => {
   if (category === "all") {
     setMenuItems(items);
     return;
   }
   const newItems = items.filter((item) => item.category === category);
   setMenuItems(newItems);
 };
 
 return (
   <main>
     <section className="menu section">
       <div className="title">
         <h2>our menu</h2>
         <div className="underline"></div>
       </div>
       <Categories  filterItems={filterItems} />
       <Menu items={menuItems} />
     </section>
   </main>
 );
}
```
> Categories.js file

```js
const Categories = ({ filterItems}) => {
 return (
   <div className="btn-container">
   <button className='filter-btn' onClick={()=>
      filterItems('all')}>
      all
      </button>
      <button className='filter-btn' onClick={()=>
      filterItems('breakfast')}>
      breakfast
      </button>
   </div>
 );
};

```


### There are two issues with this approach.
First one, we don't have a button and functionality for all, so I can't display all the items on a
Second. The even bigger issue is the fact that we are not in sync with our data.
what that means is that if I keep on adding more items and then they have different categories,
I'll always have to manually hop back to the categories and add that button. And that is not the best approach.

### To solve the problem: with the dynamic approach where we iterate over the list and then automatically get all the categories.

>App.js
So now we want to get all the values that I have in the category of property. I will set up my variable name allCategories 
The problem is that we have a repeating value.

```javascript
const allCategories = items.map((item) => item.category);
 
function App() {
 const [menuItems, setMenuItems] = useState(items);
 const [categories, setCategories] = useState(allCategories);
 ```
### To solve the problem:This is where they set their structure comes into play.
Set are a new object type with ES6 (ES2015) that allow creating collections of unique values. The values in a set can be either simple primitives like strings or integers as well as more complex object types like object literals or arrays.
The problem is that I would want to add all because of course, there also should be a button for all.
And I also want it as an array right now, and it is an object.
```js
const allCategories = new Set(items.map((item) => item.category))
```
### solve the problem
So this is where we use spread operator where I say there's going to be a new array, so all categories are going to be equal to a new array.
And then the first item will be all.
And one, I'll use this spread operator and spread out my set data structure. 

```js
const allCategories = ["all", ...new Set(items.map((item) => item.category))];
```

> Categories.js file
> Categories.js file
* Instead of manually adding those buttons, what I would want is to iterate over my categories and
then for each category, display this button and also serve the functionality where I grab
the value, the text value from the category, and add it to my filter items.
* So we're mapping over, and for each item, I would want to return a button 
The parameter I'll call this category will represent every string in my array.
And then also, since I have a list, I'm going to go with index here because we have a simple list.

* set up the onClick,  I would want to run filter items and then again pass the category. Whatever string is my value.

```js
 
const Categories = ({ filterItems, categories }) => {
 return (
   <div className="btn-container">
     {categories.map((category, index) => {
       return (
         <button
           type="button"
           className="filter-btn"
           key={index}
           onClick={() => filterItems(category)}
         >
           {category}
         </button>
       );
     })}
   </div>
 );
};
```

What's remarkable is that I get all the unique values as far as the categories in the list.
Then, once the user click, notice how we can filter those items.
If the user wants to display all, it will show all the items.
And the best thing is that now we are in sync with the data.
So that way if there are any changes in my list, it is right away represented in my app as well.
So that's my application.
