<h2 align='center'>ReactJS</h2>
<p>Библиотека которая может эффективно отрисовывать UI</p>
<hr>
<h2 align='center'>SPA - Single Page Application</h2>
<p>Одна html страница, информация подгружается 
с помощью AJAX запросов, и JS меняет страницу на месте,
перезагрузка страницы не происходит</p>
<hr>
<h2 align='center'>Функциональная компонента</h2>
<p>(презентационная, без состояния, stateless, dump, тупая )
чистая функция: принимает пропсы, отдает JSX </p>
<hr>
<h2 align='center'>Контейнерная компонента</h2>
<p>Компонента в которую заворачивается презентационная компонента
и занимается грязными вещами
Контейнерная компонента прокидывает пропсы, стейт и диспатчи в 
презентационную компоненту
В ней исполняют хуки и методы жизненного цикла
Назначение контейнерной компоненты общаться со стором</p>
<hr>
<h2 align='center'>Пропсы</h2>
<p>прокидывают в компоненты через аттрибуты 

```
  <Component name='Leo' />
  ...
  props === {
    name: 'Leo',
  }
```
Если передать просто название аргумента, то в пропсах он будет равен true<br>
```
  <Component hideBackground />
  ...
  props === {
    hideBackground: true,
  }
```
Чтобы прокинуть все пропсы<br>
```
<Component props={...props} />
```
<hr>
<h2 align='center'>React понимает массивы,</h2>
<p>поэтому можно сделать так:

```
  <div>
    <ul>
      {[1,2,3,4,5,6].map(num => <li key={num}>num</li> )}
    </ul>
  </div>
```

</p>
<hr>
<h2 align='center'>В начале</h2>
<p>

```
npx create-react-app app-name
```
```
index.js:
import React from 'react'; // везде где есть JSX
import './index.css';
ReactDOM.render(<Component>, document.getElementById('root));
```
</p>
<hr>

<h2 align='center'>CSS</h2>
<p>Создать файл CSS модуля

```
src/components/Component:
  Component.jsx
  Component.module.css
```

Затем использовать в компоненте<br>

```
  import styles from './Component.module.css'
  ...
  <... className={styles.ClassName}>....</>
```
</p>
<hr>

<h2 align='center'>Приложение должно состоять из нескольких слоев абстракции:</h2>
<p>UI - BLL - DAL
  Поток данных должен быть однонаправленным UI => BLL => DAL => server => DAL => BLL => UI<br>
  В начале разработки приложения нужно продумать управление стейтом<br>
  UI (React) - занимается только интерфейсом, должен быть максимально чистым<br>
  BLL (Redux) - ядро приложения (данные), только после изменения BLL можно менять UI<br>
  DAL (dispatch, thunks) - общается с сервером, управляется bll уровнем<br>
</p>
<hr>
<h2 align='center'>route, BrowserRouter, HashRouter</h2>
<p>
<b>Проблема:</b>
  <p>Чтобы браузер не перезагружал страницу, когда меняется ссылка,
  а вместо этого подгружал компонент.
  Система роутинга смотрит за адресной строкой, предотвращает
  стандартное поведение браузера и подгружает компонент в 
  зависимости от адресной строки</p>

<b>Решение:
  <p>Использовать системы роутинга<br>
  Установить:<br>

  ```
    npm i react-router-dom --save  //--save значит надо добавить пакет в package.json
                                  //--save-dev добавить в dev-dependencies в package.json
  ```

  Верхнюю компоненту обернуть (один раз, внутри ничего больше не заворачивать):<br>

  ```
    import {BrowserRouter} from 'react-router-dom';
    <BrowserRouter> //Чтобы внутри можно было использовать роуты
      <App />
    </BrowserRouter>
  ```

  Ссылки вместо:<br>

  ```
    <a href="/profile">Profile</a>
  ```

  Сделать:<br>

  ```
    import {NavLink} from 'react-router-dom'
    <NavLink to="/profile" activeClassName={styles.active} >Profile</NavLink> 
    // activeClassName добавляет класс к активной ссылке
    // NavLink только меняет адрес в адресной строке и делает preventDefault
    // С Route он никак не связан
  ```

  Компоненты вместо:<br>

  ```
    <Profile />
  ```

  Сделать:<br>

  ```
    import {Route} from 'react-router-dom';
    <Route path="/profile" render={() => <Profile props={...props} />}>
      // или можно вместо render использовать атрибут component
      // Если надо передавать пропсы, то можно создать обертку
      // const WrapProfile = (props) => <Profile props={...props} />
      // и передать в атрибут component обертку, или просто компонент ...
      // <Route path="/profile" component={Profile} /> // ...если не надо пробрасывать пропсы
    //Если нужен роутинг точно с таким урлом:
    <Route exact path="/profile" component={Profile} />
    //Route только следит за адресной строкой и подключает\отключает компоненты
  ```
<p>
<hr>
<h2 align='center'>withRouter</h2>
<p>High Order Component (HOC)<br>
<b>Проблема:</b><br>
  Адрес в адресной строке выступает вторым источником истины<br>
<b>Одно из решений:</b>
  Чтобы в компоненту приходили через пропсы приходили данные об адресе:<br>
  компоненту оборачиваем в withRouter из 'react-router-dom'<br>
  Тогда в компоненте в пропсы будут приходить history, location, match<br>
  Чтобы зароутить совпадение урла<br>
  нужно в вызове компоненты через Rout к урлу добавить</p>

```
  <Route path='/profile/:userId?'> render={() => <Profile />} />
  // ? означает, что параметр является опциональным
```

<p>тогда в компоненте можно получить к userID доступ через</p>

```
  const userId = props.match.params.userId;
```
<hr>
<h1>!!! Из React не надо работать напрямую с DOM !!!</h1>
<hr>
<h1>!!! Не стоит использовать рефы !!!</h1>
<p>Рефы это работа с обычным DOM элементом через React

```
  const targetElementRef = React.createRef();
  <div ref={targetElementRef}>Element</div>
  после этого обратиться можно так:
    element = targetElementRef.current
```

</p>
<hr>

<h2 align='center'>Чтобы использовать картинку в компоненте нужно</h2>
<p>1. импортнуть ее<br>

```
  import somethingImage from '../../assets/img/something-image.png';
```
<br>
2. передать ее в аттрибут src тега img<br>

```
  <img src={somethingImage} alt='something image' />
```

</p>
<hr>
<h2 align='center'>SVG можно импортировать как реакт компонент</h2>

```
  import { ReactComponent as ReactLogo } from '../../assets/img/logo.svg';
```

<p>потом просто вставлять как JSX компонент <ReactLogo /> <br>
  Так же можно менять атрибуты svg <br>
  смотреть в девтулзах на элемент, и менять атрибуты в css</p>
<hr>
<h2 align='center'>FLUX, однонаправленные поток данных</h2>
<h1>!!!UI может изменяться ТОЛЬКО если изменился state!!!</h1>
<p>На input и textarea надо вешать обработчик onChange, который при вводе будет<br>
менять стейт (глобальный или локальный), а после этого перерисовывать компонент<br>

```
  const handleFunction = (event) => {
    updateNewValue(event.currentTarget.value)
  };
  <input onChange={handleFunction} />
```

</p>
<hr>
<h2 align='center'>Subscriber / observer</h2>
<p>Pattern<br>

  <b>Проблема:</b><br>
  циклические зависимости модулей<br>
  <b>Решение:</b><br>
    1. В модуле, где нужно использовать колбэк создаем переменную для функции,<br>
       на которую хотим подписаться<br>
    2. Определяем функцию subscribe, которая принимает колбэк (observer) и <br>
       присваивает к переменной из п.1 этот колбэк<br>
    3. Вызываем subscribe, и когда нужно вызываем функцию. Profit!<br></p>
<hr>
<h2 align='center'>Dispatch & Action</h2>
<p>Action - объект, у которого как минимум есть свойство type<br>
  1. Создаем action type отдельной константой, чтобы не ошибаться потом<br>

  ```
    const PROFILE_ACTION_TYPE = 'PROFILE_ACTION_TYPE';
  ```

  2. Делаем action creator, чтобы не создавать action каждый раз руками<br>

  ```
    export const profileActionCreator = (payLoad) => ({ type: PROFILE_ACTION_TYPE, payLoad: {...payLoad} });
  ```

  3. Используем его в компоненте при вызове dispatch<br>

<b>Dispatch</b> - функция которая принимает action, пробрасывает его в reducer,<br>
и вызывает reducer</p>
<hr>
<h2 align='center'>Reducer</h2>
<p>это чистая функция, которая принимает старый state, action,<br>
если нужно применяет action к state и возвращает новый state, или старый, если он <br>
не изменился<br>

```
  const initialState = {
    id: null,
    name: null,
    ...
    something: true
  };

  const profileReducer = (state = initialState, action) => {
    switch (action.type) {
      case PROFILE_ACTION_TYPE:
        return {
          ...state,
          ...action.payLoad
        }
      default:
        return state;
    }
  }
```

</p>
<hr>
<h2 align='center'>Redux - стейт менеджер</h2>
<p>
1. <b>npm i redux --save</b><br>
2. создаем /src/redux/redux-store.js<br>
3. В redux-store.js:<br>

```
  import //всех входящих reducer
  import { createStore, combineReducers } from 'redux

  const reducers = combineReducers({
    profilePage: profileReducer,
    auth: authReducer,
    ...
  });

  export const store = createStore(reducers)
```  
</p>
<hr>
<h2 align='center'>Context API</h2>
<p>Почитать доку<br>

```
const StoreContext = React.createContext(null);
<StoreContext.Provider value={store}>
  <App />
</StoreContext.Provider>
```

Потом в дочерних контейнерных компонентах можно получить доступ<br>
к store обернув компоненту<br>

```
  <StoreContext.Consumer> // фигурная скобка должна быть на новой строке!!!!!
  { (store) => {
    const state = store.getState();
    return <MyComponent state={state}> 
    }
  }
  </StoreContext.Consumer>
```

</p>
<hr>
<h2 align='center'>react-redux</h2>

```
npm i react-redux --save
```

<p>Прослойка между react и redux<br>
Предоставляет Provider в который нужно обернуть компоненту<br>
к которой подключаем store<br>

```
  <Provider store={store}>
    <App />
  </Provider>
```

Предоставляет функцию connect<br>

```
  const mapStateToProps = (state) => ({
    componentState: state.componentState,
  });
  const mapDispatchToProps = (dispatch) => ({
    someDispatch: dispatch(someActionCreator)
  });
  const containerComponent = connect(mapStateToProps, mapDispatchToProps)(Component)
```

  Первым вызовом принимает две функции, которые отдают объекты<br>
  со стейтом и колбеками (диспатчами и thunks). Слепливает их и прокидывает в комопнент,<br>
  который передан во втором вызове<br>
  Обычно коннект пишут так<br>

```
  const containerComponent=connect(mapStateToProps, {someDispatch})(Component)
```

  при этом надо колбэки называть так же как action creator,<br>
  то есть вместо mapDispatchToProps передаем объект с action creatorами<br>
  коннект сам при вызове коллбеков сам вызывает dispatch(someActionCreator)<br>
  Каждый раз когда в стейте происходят изменения, запускается<br>
  mapStateToProps и если часть стейта которая относится к компоненте<br>
  тоже изменилась компонента перерисовывается (меняется в reducer)<br>
  Поэтому в reducer меняем только то, что надо, никаких полных копий стейта<br>
</p>
<hr>
<h2 align='center'>REST API </h2>
<p>API - Application Programm Interface<br>

Server API<br>
  1. endpoints (URL)<br>
  2. http-request types (get / post)<br>
  3. Что надо отправлять<br>
  4. Что получишь в ответе<br>
  5. http codes<br>

REST API<br>
  правила для server API<br>
  Основное:<br>
    Для работы с одинаковыми сущностями должен быть один эндпоинт<br>
    т.е. не ...ru/user/get/124124<br>
    а ...ru/user<br>
    и на него посылать разные запросы get post put delete patch<br>

  get получить, нет payload<br>
  delete удалить, нет payload<br>
  post отправить, есть payload<br>
  put обновить, есть payload<br>
</p>
<hr>
<h2 align='center'>axios </h2>
<p>
Позволяет создавать инстансы с параметрами по умолчанию<br>

```
const instance = axios.create({
  withCredentials: true, //всегда отправлять куку при кроссдоменных запросах
  baseURL: 'https://social-network.samuraijs.com/api/1.0/', //все урлы в инстансе будут с базовым
  headers: {  //заголовки
    'API-KEY': '67561d1f-451b-4fe0-acdc-46ba4874e5e5',
  },
});
```

либо в каждом http запросе вторым аргументом надо передавать объект с параметрами<br>

```
  axios.get(url, {
    withCredentials: true,
    headers: {
      ....
    }
  })
```

```
instance
  .get(url)
  .delete(url)
  .post(url, payload)
  .put(url, payload)
  .then(response => response.data)
```

С get запросом отправляют get параметры, которые добавляются в конце<br>
урла через ?. Например <b>....ru/page?id=1243&pageSize=10</b>
</p>
<hr>
<h2 align='center'>Классовый компонент</h2>
<p>

```
class SomeComponent extends React.Component {
  constructor (props) {
    super(props)          //есил конструктор только вызывает супер, его можно не писать
  }
  componentDidMount = () => {
    ...
  }
  componentDidUpdate = () => {
    ...
  }
  componentWillUnmount = () => {
    ...
  }
  render() {
    return (
      <JSX />
    )
  }
}
```

При использовании классовой компоненты реакт создает инстанс и хранит его в памяти<br>
пока компонента не будет демонтирована. Поэтому в инстансе можно хранить локальный стейт.<br>
Методы в классовом компоненте надо либо биндить в конструкторе<br>

```
constructor (props) {
  super(props)
  this.someMethod = this.someMethod.bind(this)
}
```

или объявлять стрелочными функциями<br>

```
someMethod = () => {....}
```

</p>
<hr>
<h2>locale state, setState</h2>
<p>
  <b>Проблема:</b>
  Компонента должна выглядеть по разному в зависимости от параметра, который<br>
  BLL нафиг не нужен. Например промежуточные значения в инпуте, или вид переключателя,<br>
  или положение меню и т.п.<br>
  <b>Решение:</b>
  Использовать локальный стейт компоненты<br>

  ```
  class SomeComponent extends React.Component {
    state = {
      editMode: false,
      someAdditionalProp: 'some text'
    }
    activateEditMode = () => this.setState({
      editMode: true
    })
    return () {
      <div>
        {this.state.editMode && <input type='text'/>}
        {!this.state.editMode && <span>This is a span</span>}
        <button onClick={activateEditMode}>toggle edit mode</button>
      </div>
    }
  }
  ```
  1. Объявить стейт в классовой компоненте просто свойством state<br>
  2. Менять локальный стейт только через метод setState({})<bt>
  передавая в него объект с свойствами, которые надо поменять<br>
  3. React сам изменит в стейте только те свойства, которые передали<br>
  4. setState - <b>асинхронная операция</b>, поэтому так как написано в п.2 делать не надо :)<br>
  5. а надо в setState передавать не объект, а функцию, в которую setState передаст<br>
  первым аргументом предыдущий стейт:<br>

  ```
  activateEditMode = () => this.setState((prevState) => ({
    ...prevState,
    editMode: true
  }))
  ```

</p>
<hr>
<h2 align='center'>Методы жизненного цикла</h2>
<p>
В них нужно делать сайдэффекты, например запросы на сервак, работу со стейтом<br>

<b>componendDidMount</b><br>
Вызывается один раз, когда компонента была замонтирована<br>
<b>componentDidUpdate</b><br>
Вызывается каждый раз, когда была перерендерилась<br>
<b>componentWillUnmount</b><br>
Вызывается один раз, перед тем как размонтировать компоненту<br>
</p>
<hr>
<h2 align='center'>Paginator</h2>
<p>Постраничный вызов<br>
Компонента которая создает список с нумерацией страниц порционно,<br>
и вешает на каждый номер клик по которому вызывает колбек.<br>
В колбеке передают санку, которая запрашивает с сервера нужную страницу<br>
В пагинатор передаем {totalItemsCount, pageSize, currentPage, onPageChanged, portionSize = 10}
</p>
<hr>
<h2 align='center'>Preloader</h2>
<p>
Показывать крутилку пока идет загрузка<br>
1. В стейте внести флаг isFetching, который переключается в начале запроса на true, а когда пришел ответ на false<br>
2. Если запрос при нажатии на кнопу: по этому флагу disable кнопку<br>
3. Если этот флаг true, показываем анимашку с загрузкой<br>
</p>
<hr>
<h2 align='center'>API, DAL</h2>
<p>
Выносим все функции работы с api в<br>
<b>/src/api/some-api.js<b><br>
Это DAL уровень<br>
Различные запросы инкапсулируем в объекты, а объекты экспортируем
</p>
<hr>
<h2 align='center'>Cookie</h2>
<p>
Текстовый файл, который отправляется на сервер с каждым запросом<br>
Сервер его меняет или нет и отправляет обратно<br>
Кука храниться для каждого сайта своя<br>
Кука привязывается к одному домену, на каждый домен своя кука<br>
</p>
<hr>
<h2 align='center'>Thunk</h2>
<p>
  Функця которыя лежит в BLL<br>
  UI может пинать BLL только через dispatch(action)<br>
  BLL пинает DAL чтобы тот сделал запрос на сервак и вернул ответ в BLL<br>
  BLL перерисовывает UI<br>
  Редюсеры должны работать синхронно, поэтому запросы к серверу в них использовать нельзя<br>
  Поэтому создают функцию, которая занимается асинхронными запросами, и по получению<br>
  результата диспатчит что-то в стор.<br>
  Thunk это функция которая делает асинхронную операцию и которая умеет диспатчить обычные<br>
  action<br>
  Thunk можно задиспатчить в стор и стор ее запустит. Параметры для диспатча передают<br>
  через замыкание, получается типа thunk creator<br>

  ```
    const addPost = (postText) => (dispatch) => {
      dispatch(onLoading());
      axios.post({postText}).then(() => {
        dispatch(addPost(message));
        dispatch(offLoading());
      })
    };
  ``` 
  
  <h2 align='center'>!!!стор не умеет принимать в качестве диспатча функцию, поэтому надо использовать middleWare!!!</h2>
  <h2 align='center'>middleware</h2>
  обычный поток <b>dispatch => store => reducer1, reducer2, ...reducerN</b><br>
  надо сделать так <b>dispatch => store => reducers || run thunk function</b><br>
  то есть если передать санку в санку она рекурсивно будет запускаться и запускать свои диспатчи<br>
  Чтобы задиспатчить thunkCreator, нужно:<br>
    1. <b>npm i redux-thunk</b><br>
    2. там где создаем store, в редаксовский createStore вторым аргументом передать<br> <b>applyMiddleware</b><br>

    ```
    import thunkMiddleware from 'redux-thunk';
    let reducers = combineReducers({
      auth: authReducer,
      ...
    });
    let store = createStore(reducers, applyMiddleware(thunkMiddleware))
    ```

  После этого можем прокидывать в connect вместе с dispatchами, санка будет нормально запускаться<br>
</p>
<hr>
<h2>Redirect</h2>
<p>
  <b>Проблема:</b>
  ограничить доступ к какой-то странице, например чтобы редиректить на форму логина<br>
  либо просто редиректнуть куда-нибудь по какому нибудь условию<br>
  <b>Решение:</b>
  Использовать компоненту <Redirect /><br>

  ```
  import { Redirect } from 'react-router-dom';
  if (!props.isAuth) return <Redirect to='/login' />
  ```

  На редирект для авторизации обычно делают HOC<br>
  Внутри него подключают стор через коннект, чтобы дать досуп к флагу isAuth<br>
</p>
<hr>
<h2>HOC</h2>
<p>
High Order Component<br>
Функция принимающая компоненту и отдающая контейнерную компоненту<br>

```
let HOC = (Component) => {
  let WrapperComponent = (props) => <Component bla='yo' {...props} />
  return WrapperComponent
}
```

</p>
<hr>
<h2>Compose</h2>
<p>
  <b>Проблема:</b>
  большая вложенность при оборачивании компонента в хоки, <br>
  HOC(HOC(HOC(HOC(HOC))))<br>
  <b>Решение:</b>
  приенить compose<br>

  ```
  import { compose } from 'redux';
  export default compose(
    withAuthRedirect,
    withRouter,
    connect(mapStateToProps, {dispatch, thunkCreator})
  )(Component);
  ```

</p>
<hr>
<h2>Обновление create-react-app</h2>
<p>
  1. Google it :)<br>
  2. Google it one more time :)<br>
  3. Ok, google 'create-react-app update to new version'<br>
</p>
<hr>
<h2></h2>
<p>

</p>
<hr>
<h2></h2>
<p>

</p>
<hr>
<h2></h2>
<p>

</p>
<hr>
