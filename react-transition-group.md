# React Transition Group

React Transition Group tends to be a bit finicky. Here is the code to make it work.

Wrap `<CSSTransition/>` inside `<TransitionGroup/>`. Remember to have a `key` prop on the `<CSSTransition/>`.
Whenever the `key` prop changes, `<TransitionGroup/>` will transition out the first `<CSSTransition/>` and transition in a second one without you needing to do anything.

Sample React code:
```
<TransitionGroup className={cs.animationGroup}>
  <CSSTransition
    key={this.state.index} /* whenever this.state.index changes, the icon will transition */
    timeout={{
      enter: 600,
      exit: 200
    }}
    classNames={{
      appear: cs.shapeAnimationAppear,
      appearActive: cs.shapeAnimationAppearActive,
      enter: cs.shapeAnimationEnter,
      enterActive: cs.shapeAnimationEnterActive,
      enterDone: cs.shapeAnimationEnterDone,
      exit: cs.shapeAnimationExit,
      exitActive: cs.shapeAnimationExitActive,
      exitDone: cs.shapeAnimationExitDone,
     }}
  >
    <Icon glyph={this.state.testShapes[this.state.index]} className={cs.currentShape} />
  </CSSTransition>
</TransitionGroup>
```

This will render to:
```
<div class="animationGroup-3ncjy">
  <i class="currentShape-IxugU icon-triangle-1BN4e"></i>
</div>
```

The shape animation CSS classes will be applied to the `<i/>`.

Here is some sample CSS for animations:

```

.shapeAnimationEnter {
  opacity: 0;
  transform: translateX(10px);
  transition: opacity 200ms linear 400ms, transform 200ms linear 400ms;
}

.shapeAnimationEnterActive {
  opacity: 1;
  transform: translateX(0px);
}

.shapeAnimationExit {
  opacity: 1;
  transition: opacity 200ms, transform 200ms;
  transform: translateX(0px);
}

.shapeAnimationExitActive {
  opacity: 0;
  transform: translateX(-10px);
}
```

Also, to sidestep the issue of your two child elements pushing each other round, make everything position-absolute:
```
.animationGroup {
  position: relative;
}
.currentShape {
  position: absolute;
}
```
