# Unit testing

## Test window.open using jasmine

**component.ts**

```ts
  goToLink(url: string) {
    window.open(url, '_blank');
  }
```

**spec.ts**

```ts
it('should test window open event', () => {
  spyOn(window, 'open').and.callFake(() => {
    return window;
  });
  component.goToLink('test');
  expect(window.open).toHaveBeenCalled();
});
```
