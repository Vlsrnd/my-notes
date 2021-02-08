<h2>ReactJS</h2>
<p>Библиотека которая может эффективно отрисовывать UI</p>
<hr>
<h2>SPA - Single Page Application</h2>
<p>Одна html страница, информация подгружается 
с помощью AJAX запросов, и JS меняет страницу на месте,
перезагрузка страницы не происходит</p>
<hr>
<h2>Функциональная компонента</h2>
<p>(презентационная, без состояния, stateless, dump, тупая )
чистая функция: принимает пропсы, отдает JSX </p>
<hr>
<h2>Контейнерная компонента</h2>
<p>Компонента в которую заворачивается презентационная компонента
и занимается грязными вещами
Контейнерная компонента прокидывает пропсы, стейт и диспатчи в 
презентационную компоненту
В ней исполняют хуки и методы жизненного цикла
Назначение контейнерной компоненты общаться со стором</p>
<hr>
<h2>Пропсы</h2>
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
<h2>React понимает массивы,</h2>
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
<h2>В начале</h2>
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

<h2>CSS</h2>
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

<h2>Приложение должно состоять из нескольких слоев абстракции:</h2>
<p>UI - BLL - DAL
  Поток данных должен быть однонаправленным UI => BLL => DAL => server => DAL => BLL => UI<br>
  В начале разработки приложения нужно продумать управление стейтом<br>
  UI (React) - занимается только интерфейсом, должен быть максимально чистым<br>
  BLL (Redux) - ядро приложения (данные), только после изменения BLL можно менять UI<br>
  DAL (dispatch, thunks) - общается с сервером, управляется bll уровнем<br>
</p>
<hr>
<h2>route, BrowserRouter, HashRouter</h2>
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
<h2>withRouter</h2>
<p>High Order Component (HOC)<br>
<b>Проблема:</b><br>
  Адрес в адресной строке выступает вторым источником истины<br>
<b>Одно из решений:</b>
  Чтобы в компоненту приходили через пропсы приходили данные об адресе:<br>
  компоненту оборачиваем в withRouter из 'react-router-dom'<br>
  ??? Помимо этого начинают нормально работать роуты<br>
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

<h2>Чтобы использовать картинку в компоненте нужно</h2>
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
<h2>SVG можно импортировать как реакт компонент</h2>

```
  import { ReactComponent as ReactLogo } from '../../assets/img/logo.svg';
```

<p>потом просто вставлять как JSX компонент < ReactLogo /> <br>
  Так же можно менять атрибуты svg <br>
  смотреть в девтулзах на элемент, и менять атрибуты в css</p>
<hr>
<h2>FLUX, однонаправленные поток данных</h2>
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
<h2>Subscriber / observer</h2>
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
<h2>Dispatch & Action</h2>
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
<h2>Reducer</h2>
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
<h2>Redux - стейт менеджер</h2>
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
<h2>Context API</h2>
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
<h2>react-redux</h2>

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
<h2>REST API </h2>
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
<h2>axios </h2>
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
<h2>Классовый компонент</h2>
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
  2. Менять локальный стейт только через метод setState({})<br>
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
<h2>Методы жизненного цикла</h2>
<p>
В них нужно делать сайдэффекты, например запросы на сервак, работу со стейтом<br>

<b>componendDidMount</b><br>
Вызывается один раз, когда компонента была замонтирована<br>
<b>componentDidUpdate</b><br>
Вызывается каждый раз, когда компонента перерендерилась<br>
(обновились пропсы или стейт)<br>
в componentDidUpdate приходят прошлые пропсы и стейт<br>
Поэтому чтобы не зациклить движок в при апдейте, нужно делать сравнение с прошлыми значениями<br>

```
componentDidUpdate(prevProps, prevState) {
  if (prevProps!== this.props || prevState !== state) {
    //do some code
  }
}
```

<b>componentWillUnmount</b><br>
Вызывается один раз, перед тем как размонтировать компоненту<br>
</p>
<hr>
<h2>Paginator</h2>
<p>Постраничный вызов<br>
Компонента которая создает список с нумерацией страниц порционно,<br>
и вешает на каждый номер клик по которому вызывает колбек.<br>
В колбеке передают санку, которая запрашивает с сервера нужную страницу<br>
В пагинатор передаем {totalItemsCount, pageSize, currentPage, onPageChanged, portionSize = 10}
</p>
<hr>
<h2>Preloader</h2>
<p>
Показывать крутилку пока идет загрузка<br>
1. В стейте внести флаг isFetching, который переключается в начале запроса на true, а когда пришел ответ на false<br>
2. Если запрос при нажатии на кнопу: по этому флагу disable кнопку<br>
3. Если этот флаг true, показываем анимашку с загрузкой<br>
</p>
<hr>
<h2>API, DAL</h2>
<p>
Выносим все функции работы с api в<br>
<b>/src/api/some-api.js<b><br>
Это DAL уровень<br>
Различные запросы инкапсулируем в объекты, а объекты экспортируем
</p>
<hr>
<h2>Cookie</h2>
<p>
Текстовый файл, который отправляется на сервер с каждым запросом<br>
Сервер его меняет или нет и отправляет обратно<br>
Кука храниться для каждого сайта своя<br>
Кука привязывается к одному домену, на каждый домен своя кука<br>
</p>
<hr>
<h2>Thunk</h2>
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
  
  <h2>!!!стор не умеет принимать в качестве диспатча функцию, поэтому надо использовать middleWare!!!</h2>
  <h2>middleware</h2>
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
  Использовать компоненту < Redirect /><br>

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
<h1>!Внутри отдельного рендера свойства и состояния не изменяются!</h1>
<p>Так, как будто все пропсы и стейт были вынесены замыканием в локальную область видимости<br>
</p>
<hr>
<h2>useState</h2>
<p>
  Хук создания локального стейта для функциональной компоненты

  ```
    const [count, setCount] = useState(0);

    const [profile, setProfile] = useState({
      name: '',
      age: null,
      gender: null
    })
  ```

  Принимает начальный стейт, отдает массив в котором:<br>
  первый элемент это стейт<br>
  второй элемент это функция изменения этого стейта<br>
  В функцию изменения стейта можно передавать значение<br>
  Но так как она асинхронна, лучше передавать колбэк функцию, в которую<br>
  react первым параметром передаст предыдущий стейт<br>

  ```
    setCount(prevCount => prevCount + 1)
  ```

</p>
<hr>
<h2>Redux form</h2>
<p>
  <b>Проблема:</b><br>
  Сложно выковыривать инфу из сложных форм<br>
  Сложно делать валидацию и подсветку ошибок валидации<br>
  <b>Решение:</b><br>
  Redux form добавляет в глобальный стор свой редюсер. и обрабатывает формы<br>

  1. Устанавливаем

  ```
  npm i redux-form
  ```

  2. Добавляем в редакс:

  ```
  inport { reducer as formReducer } from 'redux-form';

  const reducers = combineReducers({
    //some reducer
    //some reducer
    ...
    form: formReducer   //обязательно надо называть form
  });
  const store = createStore(reducers, applyMiddleware(thunkMiddleware))
  ```

  3. Делаем форму и передаем ее в reduxForm
  
  ```
  import { reduxForm, Field } from 'redux-form';

  const LoginForm = (props) => {
    return (
      <form>
        <div><Field component='input' name='login' placeholder='Login' /></div>
        <div><Field component='input' name='password' placeholder='Password' /></div>
        <div><Field component='checkbox' name='rememberMe' type='checkbox' /></div>
        <div><button>Login</button></div>
      </form>
    )
  };

  const LoginReduxForm = reduxForm({
    form: 'login'   //уникальное имя формы
  })(LoginForm)
  ```

  Вместо инпутов и текстарен ставят <Field /> В аттрибуты которой <br>
  передают component='тип компоненты (input, checkbox, textarea)'<br>
  и передают все аттрибуты которые нужны конечной компоненте<br>
  Т.е. <Field /> создаст элемент указанный в аттрибуте component<br>
  и присвоит ему все остальные аттрибуты кроме name<br>
  Под именем name redux-form будет добавлять значение поля в стейт<br>
  То есть при изменении чего-то в форме, в стейте в объекте form<br>
  будет создаваться объект с названием формы, в котором будет инфа по форме<br>
  !!!Работать с ним напрямую нельзя!!!<br>

  4. Чтобы получить при сабмите данные формы
  вешаем на форму 

  ```
  <form onSubmit={props.handleSubmit}>
  ...
  </form>
  ```

  handleSubmit приходит из редакс форм, когда мы оборачиваем форму см.п.3<br>
  Что происходит внутри handleSubmit:<br>
    - event.preventDefault() поэтому страница не будет перезагружаться<br>
    - собираются все данные с формы
    - вызывается props.onSubmit(formDate) //передает в него объект с собраными данными <br>

  Поэтому там где мы объявляем компонент с редакс формой, надо передать onSubmit:<br>

  ```
  const Login = (props) => {
    const onSubmit = (formData) => {
      // и тут у нас уже есть все данные формы, делай с ними шо хошь
    };

    return <LoginReduxForm onSubmit={onSubmit}>
  }
  ```

</p>
<hr>
<h2>Валидация</h2>
<p>
  Выносим в отдельный файл<br>
  Валидатор это функция которая получает value, проверяет его и если норм<br>
  возвращает undefined, если валидация не пройдена - возвращает ошибку<br>

  ```
  export const requiredField = value => value ? undefined : 'Field is required';
  export const maxLengthCreator = (max) => (value) => value.length <= max undefined : 'Too long';
  ```

  Потом надо добавить валидатор в Field (см. доку redux-form => field-level validation)

  ```
  const maxLength30 = maxLengthCreator(30); //создаем отдельно

  <Field name='login' component='input' validator={[required, maxLength30]} />
  ```

  <h2>Кастомный компонент</h2>
  Чтобы подсветить индекс, нужно создавать кастомный компонент инпута, текстарены и т.д.<b>
  В Field в аттрибут component, можно передать не строку, а компонент:

  ```
    const Input = (props) => <input........./>

    <Field name='login' component={Input} />
  ```

  Тогда редакс-форм в пропсы Input прокинет помимо атрибутов еще объекты input и meta <br>
  в meta есть свойства touched (был ли элемент тронут) и error (возникла ли ошибка) и warning<br>

  ```
  export const CreateFormElement = (element) => ({input, meta: {touched, error}, ...props}) => {
    const hasError = touched && error;
    return (
      <div className={styles.formControl + ' ' + (hasError ? styles.error : '')} >
        <div>
          {React.createElement(element, {...input, ...props})}
        </div>
        { hasError && <span>{error}</span> }
      </div>
    )
  };
  ```

<h2>stopSubmit</h2>
<b>Проблема:</b><br>
  Нет визуализации ошибки при отправлении логина<br>
<b>Решение:</b><br>
  Редакс форм предоставляет специальный action, который<br>
  можно получить из специального action creator stopSubmit<br> 
  Проверяем респонс с апи и если пришел код с ошибкой: <br>

```
  import { stopSubmit } from 'redux-form';
  if (response.resultCode !== 0) {
    const action = stopSubmit('login', {_error: data.messages[0] || 'some error'});
    dispatch(action);
  }
```

В stopSubmit первым аргументам передаем название формы, которую стопаем<br>
Вторым параметром передаем объект, в котором указываем проблемные свойства, например:<br>

```
stopSubmit('login', {email: 'Email is wrong'});
```

<b>_error</b> - специальное свойство чтобы ошибку получила вся форма<br>
Когда мы оборачиваем форму редакс-формой, в нее через пропсы прокидывается<br>
свойство error, или не прокидывается если ошибки нет<br>
поэтому в форме визуализируем ошибку по наличию этого свойства<br>
</p>
<hr>
<h2>Программный редирект</h2>
<p>
  Не стоит использовать, но есть такой вариант:

  ```
  if (some ... some) this.props.history.push('/login')
  ```

</p>
<hr>
<h2>Селекторы, reselect</h2>
<p>
  <b>Проблема</b><br>
  BLL должен сам определять структуру данных, так как удобнее и оптимальнее ему<br>
  В UI свои структуры для данных, которые удобны для него<br>
  Нужен посредник, до этого этим посредником выступал mapStateToProps<br>
  Но если структура данных в бизнесе поменяется, придется менять код во многих местах<br>
  а значит у нас дублирование кода<br>
  <b>Решение</b><br>
  Использовать селекторы в качестве дополнительного посредника<br>
  Селектор достает необходимые данные и передает их в mapStateToProps<br>

  ```
  getUsers = (state) => {
    return state.users
  }
  ```

  <b>Проблема:</b><br>
  mapStateToProps вызывается каждый раз когда в стейте что-то меняется,<br>
  Селекторы могут выполнять большую тяжелую логику<br>
  Они создавались в том числе для инкапсуляции таких вещей<br>
  mapStateToProps срабатывает часто, поэтому и селекторы вызываются часто<br>
  а если они тяжелые, то это косяк<br>
  еще сложно дебажить в селекторах, т.к. они вызываются каждый раз<br>

  <b>Решение:</b><br>
  Важно в редюсере менять только нужную часть стейта, чтобы оптимизировать перерисовки<br>
  Поэтому нельзя делать полную копию стейта в редюсерах<br>
</p>
<h2>Reselect library</h2>
<p>
  Библиотека которая сравнивает поменялся стейт или нет, и если нет, то возвращает кешированный
  результат селектора. <br>
  Реселект говорит, если у вас есть сложный селектор, то создавайте его через реселект.
  простой селектор: <br>

  ```
  export const getUsers = (state) => {
    return state.users;
  };
  ```
  
  сложный селектор:

  ```
  export const getUsers = (state) => {
    return state.users.filter(u => u.name === 'Leo')
  }
  ```

  1. `npm i reselect`

  2. Создаем селектор через реселект<br>
      Первым параметром передаем в createSelector примитивный селектор<br>
      результат выполнения которого реселект сравнит с кешированным значением<br>
      и если он отличается просунет в функцию, переданную вторым параметром<br>
      То есть мы объявляем зависимости, и реселект проверяет изменения в зависимостях<br>
      В зависимости можно передавать несколько селекторов простых или сложных<br>
      то есть в зависимости можно передавать селекторы объявленые через createSelector<br>

  ```
  import createSelector from 'reselect';

  export const getUsers = (state) =>  state.users;

  export const getUsersSuper = createSelector(
    getUsers, 
    (users) => {
    return users.filter(u => u.name === 'Leo')
  })
  ```
  
  3. Вызываем его там же где обычный стейт, вместо него, допустим в mapStateToProps

</p> 
<hr>
<h2>shouldComponentUpdate</h2>
<p>
  <b>Проблема:</b><br>
  Оптимизация перерисовки дочерних компонентов<br>
  Меняется часть компоненты => перерисовываются все дочернии компоненты<br>
  <b>Решение:</b><br>
  В классовой компоненте можно объявить метод<br>
  
  ```
  class MyPosts extends Component {
    shouldComponentUpdate(nextProps, nextState) {
      return nextProps !== this.props || nextState !== this.state
    }
    render() {
      ...
    }
  }
  ```

  Или можно расширять компонент от `PureComponent` вместо Component:

  ```
  import { PureComponent } from 'react';
  class MyPosts extends PureComponent {
    ...
  }
  ```
  
  Тогда реакт сам добавит shouldComponentUpdate <br>

  `React.memo`<br>
  HOC для замены shouldComponentUpdate <br>
  Чтобы использовать функциональные компоненты вместо классовых<br>
  Оборачиваем нашу компоненту в React.memo: <br>

  ```
  const MyPosts = React.memo(props => {
    ....
  });
  ```

</p>
<hr>
<h2>Чистая функция (pure function)</h2>
<p>
  - immutability. Чистая функция не должна мутировать то, что в нее пришло.
  - return. обязательно что-то возвращает
  - no side-effects
  - determinable / idempotent
</p>
<hr>
<h2>Tests</h2>
<p>
  Тестирование отдельных компонентов<br>

  1. Создаем рядом с файлом компонента файл ComponetName.test.js
  2. Jest

```
  it('length of posts should be incremented', () => {
  //1. подготавливаем данные test data
    let action = addPostActionCreator('test');
    let state = {...}
  //2. делаем какое-то действие action
    let newState = profileReducer(state, action);
  //3. Проверяем результат expectation
    expect(newState.posts.length).toBe(5)
  })
```

  3. запускать `npm run test`<br>
  <hr>

  google `react-test-renderer`<br>
  google `testing react components: the mostly definitive guide`<br>
</p>
<hr>
<h2>Redux-duck</h2>
<p>
  Google it<br>
  В общих словах класть в один файл action-type, action-creator, reducer.<br>
  Называть action уникальными именами, например включая проект и компонент:<br>

  ```
  const SET_USER_DATA = '/samurai-network/auth/SET_USER_DATA';
  ```

</p>
<hr>
<h2>Redux dev-tools</h2>
<p>
  1. Устанавливаем расширение в google chrome
  2. там где создаем редаксовский стор

  ```
  import { compose } from 'redux;
  const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
  const store = createStore(reducers, composeEnhancers(applyMiddleware(thunkMiddleware)))
  ```

</p>
<hr>
<h2>React.lazy && React.Suspense</h2>
<p>
  <b>Проблема:</b><br>
  Неприлично большой размер бандла<br>
  <b>Решение:</b><br>
  Разделять бандл на части<br>

  `React lazy`<br>
  Компонента будет загружена только когда пользователь к ней обратится<br>
  1. объявить импорт через `React lazy`<br>
  до:<br>

  ```
  import SomeComponent from './SomeComponent';
  ```

  после:<br>
  ```
  const SomeComponent = React.lazy(() => import('./SomeComponent'));
  ```

  2. Обернуть компоненту в `Suspense`<br>
  `Suspense` показывает лоадер, пока нужная компонента не загрузится<br>

  ```
  import { Suspense } from 'react';

  <Suspense fallback={<div>Loading...</div>}>
    <SomeComponent />
  </Suspense>
  ```

  В роутах:<br>

  ```
  <Route path='/some-component' render={() => {
    return (
      <Suspense fallback={<div>Loading...</div>}>
        <SomeComponent />
      </Suspense>
    )
  }} />
  ```
</p>
<hr>
<h2>GH-pages</h2>
<p>
  Для деплоя на gh-pages<br>

  1. `npm i gh-pages --save-dev`<br>
  2. добавить в `package.json`:<br>

    ```
    "homepage": "https://vlsrnd.github.io/some-project"

    "scripts": {
      ...
      "predeploy": "npm run build",
      "deploy": "gh-pages -d build"
    }
    ```

    homepage - адрес куда деплоить на gh-pages<br>

  3. 
  <b>Проблема:</b><br>
  BrowserRouter думает что проект лежит в корне домена `https://vlsrnd.github.io/`<br>
  А проект лежит в `https://vlsrnd.github.io/some-project`<br>
  Поэтому когда NavLink переключает адрес, теряется `some-project`<br>
  <b>Решение</b><br>
  Использовать в BrowserRouter basename из глобального объекта node js:<br>

  ```
  <BrowserRouter basename={process.env.PUBLIC_URL}>
    <SomeComponent />
  </BrowserRouter>
  ```

  <b>Второе решение:</b><br>
  Использовать HashRouter<br>
  
  ```
  <HashRouter>
    <SomeComponent />
  </HashRouter>
  ```

</p>
<hr>
<h2>

`Загрузка изображений на сайт`</h2>
<p>

  ```
    const onMainPhotoSelected = (e) => {
      const file = e.target.files[0];
      ....
    };
    <input type='file' onChange={onMainPhotoSelected} />
  ```

</p>
<hr>
<h2>

`classnames`</h2>
<p>

  Google `classnames`
</p>
<hr>