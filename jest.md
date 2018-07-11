# Useful code samples for writing Jest tests for React components.

Shallow rendering will only go one level deep, and won't work with HOCs. Use mount for full render.
```
const wrapper = shallow(
  <ImageViewer imageUrl='abc.jpg' />
)
```

Use debug to print out what was rendered.
```
console.log(wrapper.debug())
```

`containsMatchingElement` does partial attribute matching (as opposed to exact matching). It DOES do exact child matching.
```
expect(
  shallow(
    <ImageViewer imageUrl='abc.jpg' />
  ).containsMatchingElement(
    <div className='imageViewer'>
      <img src='https://www.instabase.com/abc.jpg' alt='' />
      <Loader />
      <AnnotationLayer />
    </div>
  )
).toBe(true)
```

If you don't want to type out the entire child structure, you can also assert individual elements based on class.
```
expect(wrapper.find('.files')).toHaveLength(1)
```

Simulate events. Second parameter is the event object.
```
wrapper.find('.imageViewer').simulate('mousemove', { clientX: 200, clientY: 200 })
```

Set state and get state.
```
wrapper.setState({
  svgWidth: 50,
  svgHeight: 50,
})

wrapper.state('svgWidth')
```

If you need the component to re-render, you will need to call `update`.
```
wrapper.update()
```

Use `prop` to get attributes.
Use `toContain` to do substring matching.
```
const imageTransform = wrapper.find('img').prop('style').transform
expect(imageTransform).toContain('translate(100px, 100px)')
```

Use `toBe` to do exact matching.
```
expect(wrapper.prop('value')).toBe('new value')
```

Mock return value.
```
const getZoomToFitViewParams = jest.fn()
const getZoomedViewParams = jest.fn()

getZoomToFitViewParams.mockReturnValue({
  imgCx: 50,
  imgCy: 50,
  scale: 123,
})

getZoomedViewParams.mockImplementation((_a, _b, zoomRatio) => ({
  imgCx: 100,
  imgCy: 100,
  scale: zoomRatio,
}))
```

Assert mock function calls.
```
const onChangeFn = jest.fn()
expect(onChangeFn).toBeCalledTimes(1)
expect(onChangeFn).lastCalledWith(
  expect.objectContaining({
    name: 'Point',
    type: 'point',
  })
)
```

You can mock entire modules, which will replace all exports with `jest.fn()`.
If you module uses an alias, you will need a workaround.
See [this thread](https://github.com/facebook/jest/issues/4262)
```
jest.mock('./utils')
// Verbose workaround for aliased modules. Normally jest.mock('') is sufficient.
jest.mock('~/utils/view', () => ({
  getZoomToFitViewParams: jest.fn(),
  getZoomedViewParams: jest.fn(),
  getViewerCoordsFromMouseEvent: jest.fn(),
}))
```

