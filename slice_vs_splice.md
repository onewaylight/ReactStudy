<h2>Array.prototype.slice </h2>

<h4 style="color:blue;">원본배열은 수정하지 않고 복사본을 새로운 배열 객체로 반환</h4>

slice(start[, end])

__start: 추출 시작점__
- undefined: 0부터 slice
- 움수를 지정한 경우: 배열의 끝부분 부터의 길이
- 배열의 길이와 같거나 큰 수를 지정한 경우: 빈 배열을 반환

__end: 추출을 종료할 기준 인덱스. (end를 제외하고 그 전까지의 요소만 추출한다.)__

- 지정하지 않을 경우: 배열의 끝까지 slice
- 음수를 지정한 경우: 배열의 끝에서부터의 길이를 나타낸다. slice(2, -1)를 하면 세번째부터 끝에서 두번째 요소까지 추출
- 배열의 길이와 같거나 큰 수를 지정한 경우: 배열의 끝까지 추출.
 
__반환값: 추출한 요소를 포함한 새로운 배열.__


<h3>Array.prototype.splice()</h3>

```splice() 메소드는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경한다. 이 메소드는 원본 배열 자체를 수정한다.```

```splice(start[, deleteCount[, item1[, item2[, ...]]]])```

__start: 배열의 변경을 시작할 인덱스.__

- 음수를 지정한 경우: 배열의 끝에서부터 요소를 센다.
- 배열의 길이보다 큰 수를 지정한 경우: 실제 시작 인덱스는 배열의 길이로 설정
- 절대값이 배열의 길이보다 큰 경우: 0으로 세팅

 
__deleteCount: 배열에서 제거할 요소의 수.__

- 생략 / 값이 array.length - start보다 큰 경우: start부터의 모든 요소를 제거.
- 0 이하의 수를 지정: 어떤 요소도 제거되지 않는다.

 
item1, item2, ... : 배열에 추가할 요소.

- 지정하지 않는 경우: splice()는 요소 제거만 수행한다.

 
반환값: 제거한 요소를 담은 배열. 

- 아무 값도 제거하지 않았으면 빈 배열을 반환한다.


- The __splice()__ method returns the removed item(s) in an array and __slice()__ method returns the selected element(s) in an array, as a new array object.
- The __splice()__ method changes the original array and __slice()__ method doesn’t change the original array.
- The __splice()__ method can take n number of arguments:
