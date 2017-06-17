---
layout: post
title: javascript에서의 object 깊은복사(deep copy)
---

javascript에서의 `=` 연산자를 사용하면 기본적으로 object는 복사되지 않고 참조로 값이 할당된다.

예를들면,

```javascript
var a = {
  'id': 1
};
var b = a;
console.log(a.id);		// 1
console.log(b.id);		// 1

a.id = 2;
console.log(a.id);		// 2
console.log(b.id);		// 2 (not 1)
```

위와 같은 결과가 나오게 된다.

`=` 연산자를 사용한 값 할당은 예기치 않은 문제에 맞닥뜨릴 수 있기에 필요에 따라서 _깊은 복사(deep copy)_를 사용해야 할 때가 있다. 하지만 javascript에서의 `=` 은 _얕은 복사(shallow copy)_가 되기에 이 글에서 object의 깊은 복사를 하는 몇가지 방법에 대해 설명한다.

  

- ##### JSON.stringify() & JSON.parse()

  ```javascript
  var a = {
    'id': 1
  };
  var b = JSON.parse(JSON.stringify(a));
  console.log(a.id);		// 1
  console.log(b.id);		// 1

  a.id = 2;
  console.log(a.id);		// 2
  console.log(b.id);		// 1
  ```

- ##### jQuery.extend()

  ```javascript
  var a = {
    'id': 1
  };
  var b = jQuery.extend(true, {}, a);
  console.log(a.id);		// 1
  console.log(b.id);		// 1

  a.id = 2;
  console.log(a.id);		// 2
  console.log(b.id);		// 1
  ```

- ##### 재귀(recursive)

  ```javascript
  function deepObjCopy (dupeObj) {
  	var retObj = {};
  	if (typeof(dupeObj) == 'object') {
  		if (typeof(dupeObj.length) != 'undefined')
  			retObj = [];
  		for (var objInd in dupeObj) {
  			if (typeof(dupeObj[objInd]) == 'object') {
  				retObj[objInd] = deepObjCopy(dupeObj[objInd]);
  			} else if (typeof(dupeObj[objInd]) == 'string') {
  				retObj[objInd] = dupeObj[objInd];
  			} else if (typeof(dupeObj[objInd]) == 'number') {
  				retObj[objInd] = dupeObj[objInd];
  			} else if (typeof(dupeObj[objInd]) == 'boolean') {
  				((dupeObj[objInd] === true) ? retObj[objInd] = true : retObj[objInd] = false);
  			}
  		}
  	}
  	return retObj;
  }

  var a = {
    'id': 1
  };
  var b = deepObjCopy(a);
  console.log(a.id);		// 1
  console.log(b.id);		// 1

  a.id = 2;
  console.log(a.id);		// 2
  console.log(b.id);		// 1
  ```



기호에 맞게 사용하면 될 것 같다.