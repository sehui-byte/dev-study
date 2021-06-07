### React pagination 구현하기

- **react**

- **typescript**

- **material-ui** 

  - https://material-ui.com/components/pagination/
  - https://material-ui.com/api/pagination/
  
  

`react`, `material-ui`,` typescript`를 이용하여 `pagination`을 구현해보았다.

`pagination` 같은 경우 여러군데에서 사용될 수 있으므로 공통모듈로 만들어두면 좋다. 그런데 공통모듈로 만들어보기 전에, 리액트 공부 초짜이므로 페이지네이션을 한번 직접 구현해보는 것이 좋을 것 같아  먼저 직접 구현해보았다.

---



`presentataional-container`, `sub-component` 패턴으로 구현하여, 

- **ListContainer**
  - **model**
    - **PaginationModel**
  - **context**
    - **PaginationContext**
  - **sub-componen**t
    - **Content**
      - **ContentView**
      - **ContentComponent**
    - **Pagination**

​	구조가 위와 같다.



```javascript
 <DataList>
     <DataList.Content />
     <DataList.Pagination />
 </DataList>
```



**코드는 회사 내부라이브러리를 사용하여 작성한 코드를 변경한 것이므로 오류가 있을 수 있다. 추후 테스트를 거쳐 보완 예정.*



---

- `context`를 사용하여 pagination에 필요한 정보(totalCount, page, pageCount)를 하위 컴포넌트들에서 공유될 수 있도록 코드를 작성하였다.  이렇게 `context`를 이용하면 sub-component에게 일일이 `props`로 넘겨주지 않아도 되어 편리했다.



##### PaginationContext.tsx

```javascript
import React from 'react';

export type PaginationModel = {
  offset: number; //불러올 데이터 시작점
  totalCount: number; //총 리스트 개수
  page: number;//현재 페이지
  pageCount: number; //총 페이지 수
  onChange: (event: React.ChangeEvent<unknown>, currentPage: number) => void;
};

const PaginationContext = React.createContext<PaginationModel>({
  offset: 0,
  totalCount: 0,
  page: 0,
  pageCount: 0,
  onChange: () => {},
});

export default PaginationContext;

```



---



##### Pagination.tsx

- 만약 뿌려줄 리스트가 한개도 없다면 `null`을 리턴하여 페이지네이션이 보이지 않도록 한다.



```javascript
import React, { Component } from 'react';
import { Pagination } from '@material-ui';

interface Props {
  totalCount: number; //총 리스트 아이템 개수
  page: number;//현재 페이지
  pageCount: number; //총 페이지 수
  onChange: () => void; 
}

class Pagination extends React.Component<Props> {

  render() {
    const { totalCount, page, pageCount, onChange } = this.props;

    if (totalCount === 0) {
      return null;
    }

    return (
      <Pagination count={pageCount} page={page} onChange={onChange} />
    );
  }
}

export default Pagination;
```





##### ListContainer

- container에는 로직상 필요한 부분만 들어가고, view에게 가공된 props를 넘겨주어 view에는 로직이 들어가지 않고 받은 `props`를 뿌려주는(보여주는) 역할만을 한다.
- view 소스코드는 따로 올리지 않겠다. `material-ui`사이트에서 샘플 리스트를 가져다가 렌더링해주는 것이 다이다. 필요한 props는 container에서 전달해주기 때문에 심플하다.
- *원래 코드에서는 `mobX`를 이용하여 DB에서 가져온 데이터의 state를 관리해주는 부분이 있으나 해당 부분을 여기선 삭제하였다.* 

```javascript
import React from 'react';
import { observer } from 'mobx-react';

interface Props {
  children: React.ReactNode;
  limit?: number;
}

interface State {
  offset: number;
  totalCount: number; //총 리스트 개수
  page: number;//현재 페이지
  pageCount: number; //총 페이지 수
}

@observer
class DataListContainer extends React.Component<Props, State> {
  static defaultProps = {
    limit: 3,
  };

  state: State = {
    offset: 1,
    page: 0,
    totalCount: 0,
    pageCount: 0,
  };

  async componentDidMount() {
    await this.init();
  }

  async init() {
    const { offset } = this.state;
    const { limit } = this.propsWithDefault;    
 
    //const listData = await 
      //DB에서 list로 보여줄 데이터 받아오기

    this.setState({ page: 1, totalCount: listData.totalCount });
    if (listData.totalCount % limit === 0) {
      this.setState({ pageCount: parseInt(String(listData.totalCount / limit ), 10) });
    }
    else {
      this.setState({ pageCount: parseInt(String(listData.totalCount / limit + 1), 10) });
    }
  }

  async onChange(event: React.ChangeEvent<unknown>, currentPage: number) {
    const { limit } = this.propsWithDefault;
    
    let currentOffset = 0;

    if (currentPage === 1) {
      currentOffset = 1;
      this.setState({ offset: 1 });
    }
    else {
      this.setState({ offset: (currentPage - 1) * limit + 1 });
      currentOffset = (currentPage - 1) * limit + 1;
    }

    //const listData = await ...

    this.setState({ page: currentPage, totalCount: listData.totalCount });
  }

  render() {
    const { children } = this.props;
    const childrenWithProps = React.Children.map(children, child => {
      if (React.isValidElement(child)) {
        return React.cloneElement(child, { ...this.state, onChange: this.onChange });
      }
      return child;
    });
    return childrenWithProps;
  }
}

```



- 위 코드에서 이 부분은 `children`에게 `props`를 전달하기 위한 소스코드이다.
- [이 글](https://stackoverflow.com/questions/32370994/how-to-pass-props-to-this-props-children)을 참고했다.

```javascript
  const { children } = this.props;
    const childrenWithProps = React.Children.map(children, child => {
      if (React.isValidElement(child)) {
        return React.cloneElement(child, { ...this.state, onChange: this.onChange });
      }
      return child;
    });
    return childrenWithProps;
  }
```



---



#### material-ui pagination api

- https://material-ui.com/api/pagination/

![image-20210603122437767](C:\Users\nextree\AppData\Roaming\Typora\typora-user-images\image-20210603122437767.png)

![image-20210603122517047](C:\Users\nextree\AppData\Roaming\Typora\typora-user-images\image-20210603122517047.png)