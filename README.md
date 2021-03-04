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
  expect(window.open).toHaveBeenCalledWith('test', '_blank');
});
```

## test a component method that invokes another method

**component.ts**

```ts
add-questionnaire-popup
  emitClosePopup(e) {
    this.closePopup();
  }
```

**spec.ts**

```ts
it('should call closePopup on emitClosePopup', () => {
  spyOn(component, 'closePopup');
  component.emitClosePopup('test');
  expect(component.closePopup).toHaveBeenCalled();
});
```

## Test whether a property is defined

**component.ts**

```ts
paginationParams = {
  pageSize: 20, // no. of items per page
  currentPage: 1, // current selected page in the grid
  pageSizes: [3, 5, 10, 50, 500], // dropdown option to show no. of items per page
  totalRecordCount: 0, // total number of records from the resultset without pagination
  initialPage: 1, // to set initial page when the grid is loaded initially
  showPagination: true, // to show/hide the pagination (true/false)
  showPreviousNext: true, // to show/hide next, first, previous, last buttons
  showTotal: true, // to show/hide total records text
  sort_ascending_class: 'sort_ascending', // class for ascending icon in column header
  sort_descending_class: 'sort_descending', // class for descending icon in column header
  default_sort_class: 'commonsort', // class for default sorting in column header
  sortHeader: 'helperName', // default sort key in the column header
  sortDirection: 'asc', // default sort order in the column header
  noRecordsText: 'No Records Found', // text to show when the grid contains no result
  totalRecordsText: 'Total: ', // text to show for the total records label
  tableSummary: 'Helper Listing', // Summary that needs to be given to table
  enableRowClick: 'rowClickCallBack', // method to define for row click
};
```

**spec.ts**

```ts
it('should set paginationParams', () => {
  expect(component.paginationParams).toBeDefined();
});
```

## Test a function is called in ngOnInIt

**component.ts**

```ts
ngOnInit(): void {
    this.SetupGridParameters(); // setup grid parameters
    this.GenerateHeaders(); // setup headers
```

**spec.ts**

```ts
it('should call SetupGridParameters and GenerateHeaders', () => {
  spyOn(component, 'SetupGridParameters');
  spyOn(component, 'GenerateHeaders');
  component.ngOnInit();
  expect(component.SetupGridParameters).toHaveBeenCalled();
  expect(component.GenerateHeaders).toHaveBeenCalled();
});
```

## Test subscribe method in ngOnInit()

**component.ts**

```ts
if (!this.dashboardService.getCurrentHelperIndex()) {
      this.GenerateGrid();
    }
 this.dashboardService.getHelperIndex().subscribe((value) => {
      this.helperIndex = value;
       if (this.helperIndex) {
        this.helperData = [];
        this.tableData = [];
        this.GenerateGrid();
      }
 }
```

**spec.ts**

```ts
it('should call GenerateGrid on ngOnInit', () => {
  spyOn(service, 'getCurrentHelperIndex');
  spyOn(component, 'GenerateGrid');
  component.ngOnInit();
  expect(component.GenerateGrid).toHaveBeenCalled();
});
it('should call getHelperIndex and return a value', () => {
  let value: string;
  spyOn(service, 'getHelperIndex').and.returnValue(of(value));
  component.ngOnInit();
  fixture.detectChanges();
  expect(component.helperIndex).toEqual(value);
  expect(component.helperData).toEqual([]);
  expect(component.tableData).toEqual([]);
  expect(component.GenerateGrid).toHaveBeenCalled();
});
```

## Test subscribe method on ngOnInit

**component.ts**

```ts
this.sessionService.listenPopup().subscribe((item) => {
  this.openModal();
});
```

**spec.ts**

```ts
beforeEach(async(() => {
  service = TestBed.inject(SessionService);
}));

it('should call listenPopup and return a value', () => {
  let value: any;
  spyOn(service, 'listenPopup').and.returnValue(of(value));
  spyOn(component, 'openModal');
  component.ngOnInit();
  fixture.detectChanges();
  expect(component.openModal).toHaveBeenCalled();
});
```

## Test unsubscribe method on ngOnDestroy

**component.ts**

```ts
  ngOnDestroy(): void {
    if (this.helperSubscription) {
      this.helperSubscription.unsubscribe();
    }
  }
```

**spec.ts**

```ts
it('should call unsubscribe on ngOnDestroy', () => {
  spyOn(component['helperSubscription'], 'unsubscribe');
  component.ngOnDestroy();
  expect(component['helperSubscription'].unsubscribe).toHaveBeenCalledTimes(1);
});
```

## Test whether service called or not.(issue in this test case)

**component.ts**

```ts
public openModal() {
    this.modalRef = this.modalService.show(this.elementView, {
      class: 'modal-dialog-centered',
    });
  }
```

**spec.ts**

```ts
it('modal service is called', () => {
  spyOn(service, 'show').and.callThrough();
});
```

`

## Test a property defined on if-else condition

**component.ts**

```ts
 setImage() {
    if (this.previewURL) {
      this.imgURL = this.previewURL;
    } else {
      this.imgURL = this.responseURL;
    }
  }
```

**spec.ts**

```ts
it('imgUrl should be defined when calling setImage', () => {
  component.setImage();
  expect(component.imgURL).toEqual(component.previewURL);
  expect(component.imgURL).toEqual(component.responseURL);
});
```

## Jasmine - using forEach to test that key/value pairs in array of objects are defined

Solution:

https://stackoverflow.com/questions/49758838/jasmine-using-foreach-to-test-that-key-value-pairs-in-array-of-objects-are-def/49759509

**component.ts**

```ts
 collapsePresentTable() {
    this.resolvedNotificationList.forEach((row: any) => {
      row.isExpanded = false;
    });
  }
```

**spec.ts**

```ts
it('should set to false on collapsePresentTable', () => {
  component.resolvedNotificationList.forEach((row: any) => {
    row.isExpanded = false;
    expect(row.isExpanded).toBeFalsy();
  });
});
```

## test case for switch statements

**component.ts**

```ts
getNoteType(note) {
    switch (note?.noteType) {
      case 'AddedNotes':
        return 'Added Note: ';
        break;
      case 'ReasonNotes':
        return 'Reason Note: ';
        break;
      case 'ReturnedNotes':
        return 'Returned Note: ';
        break;
      case 'ApprovedNotes':
        return 'Approved Note: ';
        break;
      default:
        return 'Note: ';
    }
  }
```

**spec.ts**

```ts
it('should return Added Note: from getNoteType ', () => {
  const note = {
    noteType: 'AddedNotes',
  };
  const Note = component.getNoteType(note);
  expect(Note).toBe('Added Note: ');
});
it('should return Reason Note: from getNoteType ', () => {
  const note = {
    noteType: 'ReasonNotes',
  };
  const Note = component.getNoteType(note);
  expect(Note).toBe('Reason Note: ');
});
it('should return Returned Note: from getNoteType ', () => {
  const note = {
    noteType: 'ReturnedNotes',
  };
  const Note = component.getNoteType(note);
  expect(Note).toBe('Returned Note: ');
});
it('should return Approved Note: from getNoteType ', () => {
  const note = {
    noteType: 'ApprovedNotes',
  };
  const Note = component.getNoteType(note);
  expect(Note).toBe('Approved Note: ');
});
it('should return Note:  from getNoteType ', () => {
  const note = {};
  const Note = component.getNoteType(note);
  expect(Note).toBe('Note: ');
});
```

---

## testing whether data are equal

**component.ts**

```ts
this.statusValues = [
  { Id: 0, value: 'Unresolved' },
  { Id: 1, value: 'Resolved' },
];
```

**spec.ts**

```ts
it('dataCurrentNotificationList and statusValues should be set on ngOnInit', () => {
  const testStatus = [
    { Id: 0, value: 'Unresolved' },
    { Id: 1, value: 'Resolved' },
  ];
  expect(component.statusValues).toEqual(testStatus);
});
```

