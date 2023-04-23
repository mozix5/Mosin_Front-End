# STEELEYE LIMITED FRONTEND ENGINEER ASSIGNMENT
## Q1. Explain what the simple List component does.
The simple List component is a React component that takes an array of items as a prop and renders a list of those items. Each item is rendered as a SingleListItem component. The SingleListItem component takes an index, a text string, a boolean isSelected that indicates whether the item is selected, and an onClickHandler function that is called when the item is clicked. When an item is clicked, the handleClick function is called with the index of the item, and the setSelectedIndex function is called with that index, updating the state of the selectedIndex variable in the List component. The list item that is selected is highlighted in green, and the others are highlighted in red.

## Q2. What problems/warnings are there with code?
1.The setSelectedIndex function is being called incorrectly. Instead of passing a new state value to setSelectedIndex, it's calling the function with the index argument in the onClick event. To fix this, we need to wrap the onClickHandler function in an arrow function, 
onClick={() => onClickHandler(index)}
2.The isSelected prop is not being passed correctly to the SingleListItem component. The isSelected prop is a boolean, but it's being passed the selectedIndex state variable, which is a number. This can cause unexpected behavior. To fix this, we need to change the isSelected prop to a boolean by comparing it to the index of the current item
isSelected={selectedIndex === index}
3.The PropTypes.array() function is not the correct way to define the shape of an array. It should be PropTypes.arrayOf(PropTypes.shape({...})). To fix this, we need to change the PropTypes definition of the items prop to
items: PropTypes.arrayOf(PropTypes.shape({
  text: PropTypes.string.isRequired,
})),
4.The map() function used in the List component's JSX should contain a unique key prop for each element being rendered. The absence of the key prop on the SingleListItem component could result in a warning or error message in the console.
5.The setSelectedIndex function in the WrappedListComponent component is not being used correctly. Instead of calling setSelectedIndex with a value, the code is calling it with a null value, which is not valid. This can be fixed by calling setSelectedIndex(-1) or some other initial value that is not null.

## Q3.Please fix, optimize, and/or modify the component as much as you think is necessary.
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

const SingleListItem = memo(({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red' }}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
});

SingleListItem.propTypes = {
  index: PropTypes.number.isRequired,
  isSelected: PropTypes.bool.isRequired,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const List = memo(({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items &&
        items.map((item, index) => (
          <SingleListItem
            key={index}
            onClickHandler={handleClick}
            text={item.text}
            index={index}
            isSelected={selectedIndex === index}
          />
        ))}
    </ul>
  );
});

List.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

List.defaultProps = {
  items: [],
};

export default List;

