# AkshatSharma_Front-End
Online assignment submission of STEELEYE LIMITED TC.27648.2024.36882.
Simple list component 

Question 1:
In this list component we are given a structure of a list in which we can add our items with red and green check mark with default value of "null"

Features:

- showcase all the items of the list
- toggle the list status with "red" and "green" as per the requirement.


Question 2:
The following problems were figured out by me:


Problem 1 = wrong function call on Onclick it should be like
> onClick={()=> {onClickHandler(index)}} 
Problems with the code 

```const WrappedSingleListItem = ({index,isSelected,onClickHandler,text}) => {
return (
<li style={{ backgroundColor: isSelected ? 'green' : 'red'}}
onClick={onClickHandler(index)}>
{text}  </li>
)};
```

Problem 2 = isSelected props must be type of number instead of boolean as it is passed as number in SingleListItem component
```
WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

```

Problem 3 = isSelected ? "green" : "red" condition in background style in li tag is wrong as it must be like - 
> isSelected === index ? "green" : "red"



Problem 4 = syntax of useState() is declared in wrong way as it will be like = 
> const [selectedIndex, setSelectedIndex] = useState();


Problem 5 = syntax error of array prop declaration

right way -
> items: PropTypes.arrayOf(PropTypes.shape({
    text: ... same as before
}))

```
WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
};
```


Question 3:

# The Following is the Optimised Code
```
 import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const SingleListItem = memo(({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected === index ? "green" : "red" }}
      onClick={() => {
        onClickHandler(index);
      }}
    >
      {text}
    </li>
  );
});

SingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.number,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired
};

// List Component
const List = memo(({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: "left" }}>
      {items?.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>
  );
});

List.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired
    })
  )
};

List.defaultProps = {
  items: [{ text: "fsddas" }, { text: "sadad" }, { text: "sdfasdfa" }]
};

export { List };

```
