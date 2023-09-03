# 이미지 지연 로딩

![](a.png)

- 위 이미지에서 보면 bundle파일과 다음으로 main, 이미지와 폰트가 다운로드 됨.
- 그리고 한동안 banner-video 파일이 pending 상태(하얀 막대)로 존재하다가 일부 리소스(main-items.jpg)의 다운로드가 완료된 후에야 다운로드되는 것을 볼 수 있다.
- **banner-video는 페이지에서 가장 처음으로 사용자에게 보이는 콘텐츠임. 가장 나중에 로드되면, 사용자 경험에 좋지 않다.**

### 위와 같은 상황 어떻게 해결 ?

- 동영상 다운로드를 방해하는, **당장 사용되지 않는 이미지를 나중에 다운로드 되도록** 해서 동영상이 먼저 다운로드 되게 하는 것 => **이미지 지연 로드**

* 그럼 이런 이미지들 언제 로드해야할까 ?
  - 뷰포트에 이미지가 표시될 위치까지 스크롤되었을 때 이미지를 로드할지 말지 판단 가능

## Intersection Observer

```

const options = {
	root : null,
	rootMargin : '0px',
	threshold : 1.0,
}

const callback = (entries, observer) => {
	console.log('Entries', entries);
}

const observer = new IntersectionObserver(callback, options);

observer.observe(document.querySelector('#target-element1'));
observer.observe(document.querySelector('#target-element2'));
```

- options
  - root : 대상 객체의 가시성을 확일할 때 사용되는 뷰포트 요소, default는 null로 null 설정 시 브라우저의 뷰포트로 설정
  * rootMargin : root 요소의 여백, root의 가시 범위를 가상으로 확장하거나 축소 할 수 있다.
  * threshold : 가시성 퍼센티지, 대상 요소가 어느 정도 보일 때 콜백을 실행할지 결정, 1.0이면 대상 요소가 모두 보일 때 콜백 실행, 0으로 설정하면 1px라도 보이는 경우 콜백이 실행

* callback
  - 첫번째 인자(entries) : 가시성이 변한 요소를 배열 형태로 전달
