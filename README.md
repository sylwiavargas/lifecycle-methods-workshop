# Lifecycle methods

This is a repo for a lecture-workshop I gave on React 16 lifecycle methods. I am using a codebase already known to students, namely [Hogwarts lab](https://github.com/learn-co-students/hogwarts-dumbo-web-120919).

This repo contains three folders:
- `starter_kit` for those who want to code-along;
- `lecture_kit` which is the code I will use in the lecture (visible in the video);
- `done_kit` in case we don't have time to cover all bonuses and you'd like to see how certain features/methods can be implemented.

To start your adventure, `cd` into a folder of choice, run `npm install` and then `npm start`.

---

## SBATs:
- student will understand what lifecycle methods are;
- student will understand when lifecycle methods fire up;
- student will understand why **we should never put setState() in render()**;
- student will know how to prevent unnecessary rerendering of a component;
- student will understand a memory leak causes and treatment;
- student will remember forever **not to put props in state**;

---

## Resources:
[Lifecycle methods - React docs](https://reactjs.org/docs/state-and-lifecycle.html)
[Lifecycle methods graph](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)
[Lecture video - to be added](https://youtu.be/)

---

## Component tree and wireframe
![Component tree](done_kit/awwboard.png)

---

## Agenda
This lecture is meant as a space for exploring the superpowers of React components. The lecture will have a form of quick-pace fun riddles and will include practice exercices. **There are no awesomeness points and mistakes are viewed as a part of learning**.
1. **Intro + rules:**
- readme overview
- time and break
- speaking and questions
- parking lot
2. **Builder analogy:**
- explore it [here](http://lifecycle-photostory.surge.sh/)
3. **First hurdle:** logging spree
- `constructor` 
- `render`
- `componentDidMount`
4. **Break** (5 mins)
5. **Second hurdle:** SEO and accessibility
- `componentDidMount`
5. **Third hurdle:** timer
- `componentDidMount`
6. **Fourth Hurdle:** preventing memory leakage
- `componentWillnmount`
7. **Fifth Hurdle:** controlling rerendering
- `shouldComponentUpdate`
8. **Sixth Hurdle:**
- `getDerivedStateFromProps`

---
## First hurdle: logging spree
In this hurdle, your task is to put a console.log in `constructor`, `render`, `componentDidMount`, `componentDidUpdate`, `shouldComponentUpdate` and `componentWillUnmount` methods in: `App.js`, `HogContainer.js` and `PigTile.js`. Once you've done that, run your app and see what order the components are being rendered.

### styling logs
As you see, that's a lot of logs. At the top of each component, I have provided a color unique for this component. Perhaps you want to color-code your messages? Here's how to do it:
```
console.log(`c%Hello`, `color: ${nameOfTheVariable}`)
```
Of course, you can provide your own color ideas or change styling as you wish (e.g. backgroundColor or fontSize)

---

## Second hurdle: SEO and accessibility
Have you noticed that majority of react pages have the same title (visible on the tab)? That's because in `index.html` it has been declared this way and never updated. Wouldn't it be nice if you could see the subpage title at any given time? That will also boost the SEO and the screen reader experience. Here's pseudo-code version of what you want to do:
- in `componentDidMount` you want to overwrite the title, or add to the current one with this syntax:
```
document.title = "some name"
```
Add document title in `App`, `HogContainer` and `PigTile`.

---

## Third hurdle: timer
Imagine you want to have a countdown for each hog for some reason. In this hurdle, you will build a timer that count down. Here's pseudo-code version of what you want to do:
- in `componentDidMount` set an interval that will update the state every second;
- comment out previous console.logs in `render` and `componentDidUpdate`;
- console.log state.timeToGo in `render` with a name of the hog:
```
console.log(`%c${this.props.hog.name}:`, `color: ${generalTileColor}`, this.state.timeToGo)
```

### set interval syntax
```
this.interval = setInterval(
            () => this.setState({
              timeToGo: this.state.timeToGo - 1
            }),
            1000
          )
```

---

## Fourth hurdle: memory leakage
With the timer running, click on "greased". You will see this error:
> proxyConsole.js:56 Warning: Can't perform a React state update on an unmounted component. This is a no-op, but it indicates a memory leak in your application. To fix, cancel all subscriptions and asynchronous tasks in the componentWillUnmount method.

That's because you have an async method of setInterval running in the background -- you need to terminate it! 
- go to `componentWillUnmount()`;
- clear interval:
```
clearInterval(this.interval)
```

---

## Fifth Hurdle: controlling rerendering
Let's say you want the HogContainer to only update if the number of hogs is not smaller than 14. You could use `shouldComponentUpdate(nextProps, nextState)` method and write an expression that will return a boolean, for instance:
```
return nextProps.hogs.length > 14
```
Now, click "greased". Nothing changes. You have disabled updates on this component.


For fun, you can go now to your PigTile and use `shouldComponentUpdate` again. Say you want the countdown stop at 0. Put this expression in:
```
return nextState.timeToGo > -1
```
Now, run the page and observe console.logs. After a second click and unclick "greased". That restarts some tiles countdown. However, as you see, the updates stop for all tiles as soon as the first one hit 0.

---

## Sixth Hurdle: forbidden knowledge
React newbies tend to put props in state. Whoa, there's even a method that allows for that! Let's explore it.
- go to `HogContainer`;
- paste this method:
```
    static getDerivedStateFromProps(nextProps) {
         const code = nextProps.code
         return {code}
    }
```
Feel judged by Dan Abramov.

**NOTE:** in all seriousness, you should never put your props in your state. React won't yell at you but your instructors and employers will. Do not do that. This method is for a very specific edge case: while dealing with a transition animation library, you need to keep track of the window size. 

---

## NOT COVERED: getSnapshotBeforeUpdate
This method is only used in cases when you're creating an infinite scroll and want to always have the pointer at the same place without jumping to the top.

---

## BONUS: error handling
[Code along](https://blog.bitsrc.io/understanding-error-boundaries-in-react-e58f15ae1f38)