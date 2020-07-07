---
layout: post
title:  "redux-thunk"
# date:   2020-04-22 16:10:37 +0900
categories: react
---

# Redux-thunk

`Redux` 에서, `Redux-thunk` 라는 libaray 를 이용하여, 'Promise 기반 함수'를 이용하는 `Action`의 상태를 관리하는 방법을 알아보자.

# 1. Thunk library 설치

```bash
$ npm install --save redux-thunk
or
$ yarn add redux-thunk
```

# 2. Thunk middleware 추가

redux-thunk 를 middleware 에 추가하여 Action 이 직접 dispatch 를 조작할 수 있다.

```jsx
// src/index.jsp
//...
import ReduxThunk from 'redux-thunk'
//...
const store = createStore(
    reducer,
    composeWithDevTools(
        applyMiddleware(logger, ReduxThunk)
    ),
);
//...
```

※ composeWithDevTools) [https://www.notion.so/ppojin/redux-devtools-redux-middleware-f17922a15192417ab5a381be7ec93c95](https://www.notion.so/ppojin/redux-devtools-redux-middleware-f17922a15192417ab5a381be7ec93c95)

# 3. Redux 작성

## 3.2. Action

Action이 상태`[Pending, Fail, Success]`에 따라서 Reducer에 ActionName과 Data를 Dispatch한다.

```jsx
// Actions
const getResultAPI = async (data) => {
    return axios.get("/sapmleUrl", data);
}
export const getResult = (data) => {
    return dispatch => {
        
        dispatch({
						type: "entityName/FETCH_ENTITY_LOADING",
				})
        
        return getResultAPI(data)
            .then(response => {
                dispatch({
                    type: "entityName/FETCH_ENTITY_SUCCESS",
                    payload: response
                })
            }).catch(error => {
                dispatch({
                    type: "entityName/FETCH_ENTITY_FAILURE",
                    payload: error
                })
            })
    }
}
```

## 3.3. Reducer

Reducer 가 Action Name 에 따라 상태를 구분하여 Store 에 저장한다

```jsx
// store의 초기값
const initialState = {
    loading: false,
    errorMessage: null,
    updateSuccess: false,
    result: "",
}
// Reducer
export default (state = initialState, action) => {
    switch (action.type){
        case "entityName/FETCH_ENTITY_LOADING": // 요청 직후에 Store 에 저장되는 값
            return {
                ...state,
                errorMessage: null,
                updateSuccess: false,
                loading: true,
            };
        case "entityName/FETCH_ENTITY_SUCCESS": // 요청이 성공했을 때 Store 에 저장되는 값
            const result = action.payload.data;
            return {
                ...state,
                loading: false,
                result,
            };
        case "entityName/FETCH_ENTITY_FAILURE": // 요청이 실패했을 때 Store 에 저장되는 값
            return {
                ...state,
                loading: false,
                updateSuccess: false,
                errorMessage: action.payload,
            };
        default:
            return state;
    }
}
```

# 4. 작동 예시

### 4.1. Action 작동순서

1. Action 요청

    ```jsx
    export default ({getResult}) => {
    	const data = {
    		/* some initial data */
    	}
    	return (
    		<button onClick={() => getResult(data)}>
    			Action 요청
    		</button>
    	)
    }

    const mapDispatchToProps = ((dispatch) => {
        return bindActionCreators({getResult}, dispatch)
    })
    export default connect(null, mapDispatchToProps)(SectionDropZone);
    ```

2. dispatch 작동
    1. `entityName/FETCH_ENTITY_LOADING`

        getResult 가 실행될 때 지정된 disaptch(`entityName/FETCH_ENTITY_LOADING`)가 reducer 에 전달

        ```jsx
        {
          entityName: {
            result: '',
            errorMessage: ''
        		isLoading: true
          },
        }
        ```

    2. `entityName/FETCH_ENTITY_SUCCESS`

        Action 이 요청되었을 때 지정된 disaptch(`entityName/FETCH_ENTITY_SUCCESS`)가 reducer 에 전달

        ```jsx
        {
          entityName: {
            result: 'example Data',
            errorMessage: ''
        		isLoading: false
          },
        }
        ```

3. Store 이용

    ```jsx
    export default ({result, loading}) => {
    	return (
    		<div>
    			result= {result}
    			loading= {loading}
    		</div>
    	)	
    }

    const mapStateToProps = ({pender, megaVoice}) => ({
        loading: entityName.isLoading,
        result: entityName.result
    })
    export default connect(mapStateToProps,null)(EngineSTT);
    ```