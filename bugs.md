## Error: No provider for router” while writing Karma-Jasmine unit test cases

Solution: https://stackoverflow.com/a/41029925

## NullInject Error - NO provider for ToastrConfig

Solution:

**spec.ts**

```ts
import { ToastrModule, ToastrService } from 'ngx-toastr';
imports: [ToastrModule.forRoot()];
providers: [ToastrService];
```

## NullInject Error - No provider for Bsmodalservice

Solution:

**spec.ts**

```ts
import { BsModalService } from 'ngx-bootstrap/modal';
  TestBed.configureTestingModule({
      providers: [
        {
          provide: BsModalService,
          useValue: {
            show: () => ({class: 'desclaimer-popup'})
          }
        }
      ];
  })
```

## NullInject Error - No Provider for HttpClient

```ts
import {
  HttpClientTestingModule,
  HttpTestingController,
} from '@angular/common/http/testing';
import { HttpClient } from '@angular/common/http';

let httpClient: HttpClient;
let httpTestingController: HttpTestingController;

beforeEach(async(() => {
  TestBed.configureTestingModule({
    imports: [HttpClientTestingModule],
  }).compileComponents();
  httpClient = TestBed.inject(HttpClient);
  httpTestingController = TestBed.inject(HttpTestingController);
}));
```
