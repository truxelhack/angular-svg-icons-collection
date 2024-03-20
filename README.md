# Angular SVG Icons Collection

A scalable and easy-to-use collection of SVG icons for Angular applications. This guide assumes you have an Angular service or component set up to manage and expose SVG icons in your application.

## Installation

1. **Add the SVG Icons Service**: Integrate the `SvgIconsService` (you need to create this service in your Angular application) that manages the retrieval and rendering of SVG icons.

2. **Place your SVG icons**: Ensure all your SVG icons are placed in a suitable directory within your assets folder, e.g., `src/assets/icons`.

## Creating the SVG Icons Service

1. Generate a new service in your Angular application:

```bash
ng generate service services/svg-icons
```


```ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class SvgIconsService {
  constructor(private http: HttpClient) {}

  getIcon(name: string): Observable<string> {
    return this.http.get(`/assets/icons/${name}.svg`, { responseType: 'text' });
  }
}
```

## Usage

```ts
import { Component, OnInit } from '@angular/core';
import { DomSanitizer, SafeHtml } from '@angular/platform-browser';
import { SvgIconsService } from './services/svg-icons.service';

@Component({
  selector: 'app-icon-component',
  template: '<div [innerHTML]="iconSvg | safeHtml"></div>',
})
export class IconComponent implements OnInit {
  iconSvg: SafeHtml;

  constructor(private svgIconsService: SvgIconsService, private sanitizer: DomSanitizer) {}

  ngOnInit() {
    this.svgIconsService.getIcon('example-icon').subscribe(svg => {
      this.iconSvg = this.sanitizer.bypassSecurityTrustHtml(svg);
    });
  }
}
```
