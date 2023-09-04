# Expense  Tracker in ReactJS

![ss1](https://github.com/nikhilgithub911/ExpenseTracker_reactAssignment/assets/68798803/b423db70-7a0c-4465-8b3a-d5191f2a4069)

Expense Tracker is a ReactJS web app where you can deal with your expenses.
## Folder Structure
![s](https://github.com/nikhilgithub911/ExpenseTracker_reactAssignment/assets/68798803/25dec290-6ea6-49f6-ab43-b41e566c9674)


## Run

```bash
npm start
```
#### Further Content LOading ...
## Parent Component (App.jsx)

```react
import './App.css';
import Form from './Components/Form/Form';
import Expense from './Components/Expense/Expense';
import { useState } from 'react';

function App() {
  const [expenses, setExpenses] = useState([]);

  const addExpenseHandler = (expenseData) => {
    setExpenses((prevExpenses) => [...prevExpenses, expenseData]);
  };
  return (
    <div className="App">
    <h1 className='heading'><span>Nikhil's</span> Expense Tracker</h1>
    <hr className='horizon'/>
      <Form onUpdate={addExpenseHandler} />
      <Expense expenses={expenses} />
    </div>
  );
}
export default App;



```

## Working

Required styling is added in the App.css file . It has two sub components : <Form/> and <Expense/> .
UseState hook is used to manage state . Here it is used to add any new expenses to the list.
addExpenseHandler function is defined whose work is to add the Inside the addExpenseHandler function, the setExpenses function is used to update the expenses state array. It uses the useState hook from React to manage the state. in a list preserving the old ones .
 The expenseData parameter represents the data of the newly added expense.The setExpenses function takes a callback function as an argument, which is executed with the previous state value as its argument. This is done to ensure that the state updates are based on the previous state and avoid any potential race conditions.Within the callback function, the spread operator (...) is used along with the previous expenses array (prevExpenses). This creates a new array containing all the elements from the previous expenses array.To this new array, the expenseData is added, effectively appending the new expense to the end of the array.The callback returns the updated array, which is then used by the setExpenses function to update the state. As a result, React re-renders the component with the updated list of expenses.After that coming to the JSX part we have a heading and a horizontal line and two of its ubcomponent are rendered here.addExpenseHandler function will be called when form data will be updated and the newly added expenses is passed as a prop to the Expense component

## Parent Component (App.css)

```react
.App {
  text-align: center;
  background-color: #4e4e50;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  
}
.horizon{
  width: 7%;
  background-color:rgb(219, 143, 219) ;
  height: 5px;
  border: none;
  margin-top: 0;
  margin-bottom: 14px;
}
.heading{
  color: white;
}

span{
  color: rgb(219, 143, 219);
}

.under{
  height: 2px ;
  width: 4px;
}
.colored-hr {
  background-color: rgb(219, 143, 219);
  height: 5%;
  border: none;
  width: 30%;
}




```
## Child Component (Expense.jsx)

### Imports:
```
import React, { useState, useEffect } from 'react';
import './Expense.css';
import ItemDetails from '../ItemDetails/ItemDetails';

const monthNames = [
  'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
  'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'
];
```

### Expense function in bits
#### UseState hooks
``` 
const [selectedYear, setSelectedYear] = useState(2023);
const [yearTotalExpenses, setYearTotalExpenses] = useState([]);
```
The selectedYear variable is used to keep track of the currently selected year from the dropdown options. It allows the user to filter and display expenses for a specific year.
The yearTotalExpenses state variable is responsible for storing the height ratios of the expense bars in the UI for each month of the selected year. These height ratios are calculated based on the total expenses for each month.

#### useEffect hook
```
useEffect(() => {
    // Calculate total expenses for each month of the selected year
    const yearMonthsTotal = Array(12).fill(0);

    expenses.forEach((expense) => {
      const expenseYear = parseInt(expense.date.split('-')[0], 10);
      const expenseMonth = parseInt(expense.date.split('-')[1], 10) - 1;

      if (selectedYear === '' || expenseYear === selectedYear) {
        yearMonthsTotal[expenseMonth] += parseFloat(expense.amount);
      }
    });

    // Find the highest amount for scaling
    const maxExpense = Math.max(...yearMonthsTotal);

    // Calculate height ratios for each month
    const heightRatios = yearMonthsTotal.map((expense) => {
      if (maxExpense === 0) {
        return 0;
      }
      return (expense / maxExpense) * 100;
    });

    setYearTotalExpenses(heightRatios);
  }, [expenses, selectedYear]);
```
An array is initialized with 12 undefined values and then the .fill(0) adds 12 zeros,
then it is getting the expenses array as props from App.jsx .It is iterating over the expenses array with the the help of iterator (expense),then it is extracting the year from the expenses year and also the month 
selectedYear === '': This part of the condition checks if no year is selected. An empty selectedYear indicates that the user hasn't selected a specific year from the dropdown. In this case, the condition evaluates to true, because if no year is selected, you want to consider all expenses regardless of their year.
expenseYear === selectedYear: This part of the condition checks if the year of the current expense (expenseYear) matches the selected year (selectedYear). If it matches, the condition evaluates to true, meaning you want to consider this expense for calculation. 

then max expense gets the highest expense of a particular year,

yearMonthsTotal.map((expense) => { ... }): This uses the .map() function to iterate over each element in the yearMonthsTotal array. For each element (representing a month's total expense), the provided arrow function is executed.

if (maxExpense === 0) { return 0; }: This checks if the maxExpense is equal to 0. If it is, it means there are no expenses, and there's no need to calculate the ratios. In this case, a height ratio of 0 is returned for all months to avoid division by zero.

yearMonthsTotal.map((expense) => { ... }): This uses the .map() function to iterate over each element in the yearMonthsTotal array. For each element (representing a month's total expense), the provided arrow function is executed.

if (maxExpense === 0) { return 0; }: This checks if the maxExpense is equal to 0. If it is, it means there are no expenses, and there's no need to calculate the ratios. In this case, a height ratio of 0 is returned for all months to avoid division by zero.


 setYearTotalExpenses(heightRatios);
This line uses the setYearTotalExpenses function to update the yearTotalExpenses state variable with the calculated height ratios. The setYearTotalExpenses function is a setter function provided by the useState hook that allows you to update the value of yearTotalExpenses and trigger a re-render of the component.
}, [expenses, selectedYear]);
This line is part of the useEffect hook and specifies the dependencies for the effect. The effect will be triggered whenever the values of expenses or selectedYear change. In other words, whenever new expenses are provided or the selected year changes, the effect will run.

## Child Component2 (Form.jsx)
### Imports

```
import React, { useState } from 'react';
import './Form.css';

const Form = ({ onUpdate }) => {
  const [userData, setUserData] = useState({
    enteredName: '',
    enteredAmount: '',
    enteredDate: '',
  });
```
Initially  the entered data will have no value ,the form component is being called in the app component  :  
<Form onUpdate={addExpenseHandler} />
now we can call the addExpenseHandler function from its child component (form.jsx) by onUpdate(updatedExpenses).


```
  const handleExpense = () => {
    const updatedExpenses = {
      name: userData.enteredName,
      amount: parseFloat(userData.enteredAmount).toFixed(2),
      date: userData.enteredDate,
    };
    onUpdate(updatedExpenses);

    // Clear input fields after adding expense
    setUserData({
      enteredName: '',
      enteredAmount: '',
      enteredDate: '',
    });
  };
``` 
the updated expense object (updatedExpenses) is passed as on arguement through onUpdate() which is in the parent component and then which will update the expenses array.And the rest of the functions are used to handle the data changes .


##### Notes
```
Controlled Component: In React, an input element becomes a controlled component when its value is managed by React's state. In this case, the userData.enteredAmount value is being used to control the input's value.

Two-Way Binding: By setting the value attribute to userData.enteredAmount, you establish a two-way binding. This means that the value displayed in the input is directly linked to the userData.enteredAmount state value, and any changes in the input value will update the state, and vice versa.

Preserving State: Without the value attribute, the input would be uncontrolled, and the input's value would not be linked to the state. This could lead to inconsistencies between the displayed value and the actual state. By using the value attribute, you ensure that the input always reflects the current state value.

User Interaction: When the user types in the input field, the onChange event handler (handleAmountChange in this case) is triggered. This updates the userData.enteredAmount state, which then updates the value of the input, keeping the input and state in sync.
```

### Final Component (ItemDetails.jsx)

```
  <ItemDetails
            key={index}
            month={expense.date.split('-')[1]}
            year={expense.date.split('-')[0]}
            day={expense.date.split('-')[2]}
            title={expense.name}
            amount={expense.amount}
            monthNames={monthNames}
          />
```
ItemDetails is called inside the expenses component with the following passed props
which is destructered inside itself 

```
import React from 'react';
import './ItemDetails.css';

const ItemDetails = ({ month, year, day, title, amount, monthNames }) => {
  return (
    <div className='itemBucket'>
      <div className='details'>
        <div className="expenseDate">
          <span className='tc'>{monthNames[parseInt(month, 10) - 1]}</span>
          <span className='tc'>{year}</span>
          <span className='tc'>{day}</span>
        </div>
        <span className="txt">{title}</span>
        <div className="price">${amount}</div>
      </div>
    </div>
  );
}

export default ItemDetails;

```
This contains the list of the item that is being rendered 
```
monthNames[parseInt(month, 10) - 1]:
```
parseInt(month, 10): This converts the month prop (which might be a string) into an integer using base 10. For example, if month is '02', parseInt(month, 10) would become 2.
Since array indices are zero-based and months are one-based (January is 1, February is 2, and so on), subtracting 1 is needed to match the correct index in the monthNames array.

# Final Summary
```
The "Expense Tracker" is a ReactJS web application designed for managing expenses. It has a parent component, `App.jsx`, that serves as the main component. The app utilizes two child components, `Form.jsx` and `Expense.jsx`, to manage and display expense-related data.

The main functionalities are as follows:

1. **Adding Expenses:**
   - The `Form` component manages input fields for adding expenses, including name, amount, and date.
   - The `handleExpense` function is used to gather the input data and create an `updatedExpenses` object.
   - The `onUpdate` prop is used to pass the `updatedExpenses` object to the parent component (`App.jsx`).
   - The `setUserData` function is then used to clear the input fields after adding an expense.

2. **Displaying Expenses:**
   - The `Expense` component handles the display of all expenses.
   - It uses the `yearTotalExpenses` state variable to store height ratios for expense bars based on total expenses for each month.
   - The `useEffect` hook calculates the total expenses for each month of the selected year and updates `yearTotalExpenses`.
   - The `selectedYear` state variable keeps track of the chosen year for filtering expenses.

3. **UI Styling:**
   - The app's appearance is styled using CSS in the `App.css` file.
   - Styling elements include a colored horizontal line, headings, and background colors.

4. **Expense Item Details:**
   - The `ItemDetails` component is used to display individual expense items.
   - It receives various props such as `month`, `year`, `day`, `title`, `amount`, and `monthNames` to render the details.

The app's structure allows users to add and view expenses. The `Form` component gathers expense data and communicates with the parent component (`App.jsx`) to update the list of expenses. The `Expense` component handles the display of expenses, including monthly bar graphs based on total expenses. The `ItemDetails` component renders individual expense items.

For a complete understanding and implementation, refer to the provided code and descriptions.
```
