# Dump of commonly used code samples.

Shallow rendering will only go one level deep, and won't work with HOCs. Use mount for full render.
```
const wrapper = shallow(
  <ImageViewer imageUrl='abc.jpg' />
)
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

Need to explicit call re-render.
```
wrapper.update()
```

Use 'prop' to get attributes.
Use 'toContain' to do substring matching.
```
const imageTransform = wrapper.find('img').prop('style').transform
expect(imageTransform).toContain('translate(100px, 100px)')
```

Use 'toBe' to do exact matching.
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
expect(onChangeFn.mock.calls.length).toBe(1)
expect(onChangeFn.mock.calls[0][0]).toBe('new value 3')
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

