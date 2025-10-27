## Tech Stack
- ReactJS
- NextJS - pages directory and static site generation
- Tailwinds v4
- lucide-react - icons

## Layout
It is preferred to use stacking layouts using flex-box columns to avoid running out of horizontal
room. Avoid long names in buttons and prefer to use icons for buttons.

If buttons display text DO NOT give the button a static width.

## Static site generation
The frontend of the app uses static site generation so no server side rendering should be used. All
pages are stored in the pages directory and the NextJS app router is NOT used.

## Exports
Always use named exports unless explicitly told otherwise or required such as when creating NextJS
pages.


## Full screen screens
The `useFullPage` hook can be used to display a full screen page without the main navigation bar.

example:
``` tsx
import { useFullPage } from "@/lib/hooks";

function ExampleComponent(){

    useFullPage();

    return (
        <div></div>
    )
}
```

## Form Data
When creating forms store form data in a typed useState variable.

Zod schemas can be imported from `@/lib/schema` to validate types stored in the database.

Form state example:
``` tsx

interface NewsletterForm
{
    name:string
    email:string;
}
function ExampleComponent()
{
    const [newsletterData,setNewsletterData]=useState<NewsletterForm>({
        name:'',
        email:'',
    });

    return (
        <form>
            <input
                placeholder="Enter name"
                value={newsletterData.name}
                onChange={e=>setNewsletterData({...newsletterData,name:e.target.value})}
            />
            <input
                placeholder="Enter email"
                value={newsletterData.email}
                onChange={e=>setNewsletterData({...newsletterData,email:e.target.value})}
            />
        </form>
    )
}
```

## Pages
When creating NextJS pages export the page component as a default function with the function
name reflecting the name of the page.

Do not use the MainLayout component when creating a page. The Main Layout component will be
used by the top level App component.

Include the name of the page in the className of the root element of the page component using the
format of: "page--{PageComponentName}"

Example Page with a route of "/example":
``` tsx

export default function ExamplePage(){

    return (
        <div className="page--ExamplePage">
            Example page content
        <div>
    )
}
```

## Main Layout
The `MainLayout` component is used by the top level `App` component to render the main layout of the
app. By default the MainLayout should render pages in a centered column with a navigation bar.

### Main Layout Display modes
Pages can use the `useFullPage` and `useNoMargins` hooks to alter the way the page is displayed.
Implement display modes using css or class names, DO NOT change the render order or do anything
that would cause the page to be unmounted.

### Main Layout Fullscreen Mode
Pages can request to enter into fullscreen. Use the `useIsInFullPageMode` hook imported from 
`@/lib/hooks` to check if the page should be displayed in fullscreen mode. If useIsInFullPageMode
returns true hide the main navigation and any other UI other than the page content.

### Main Layout No Margins Mode
Pages can request to remove all page margins so that they can display content edge to edge. Use
the `useIsNoMarginMode` hook imported from `@/lib/hooks` to check if the page should be displayed
in no margins mode.


## Packages
This is the package.json file for the project. You can only use libraries based on the dependencies
of the package.json file.

``` json
{
  "dependencies": {
    "lucide-react": "^0.544.0",
    "markdown-it": "^14.1.0",
    "next": "15.5.4",
    "react": "19.1.0",
    "react-dom": "19.1.0",
    "rxjs": "^7.8.2",
    "uuid": "^13.0.0",
    "zod": "^4.1.11"
  },
  "devDependencies": {
    "@tailwindcss/postcss": "^4",
    "@types/node": "^20",
    "@types/react": "^19",
    "@types/react-dom": "^19",
    "tailwindcss": "^4",
    "typescript": "^5"
  }
}


```

## Standard Components
The following components can be used

### Logo
Displays the apps logo using an SVG

Import: `import { Logo } from "@/components/Logo";`
Props:
``` ts
interface LogoProps
{
    color?:string;
    /**
     * @default "w-8 h-8"
     */ 
    className?:string;
    size?:string|number;
}
```

## Utility Functions
The following utility functions can be imported from `@/lib/util`

``` ts
export type ClassNameValue = string | false | number | null | undefined | {
    [className: string]: any;
} | ClassNameValue[];

/**
 * Combines class names and ignores falsy values.
 */
export const cn=(...classNames:ClassNameValue[]):string=>;
```


## Hooks
React components can use the following hooks imported from `@/lib/hooks`

``` ts
/**
 * Hides common UI controls such as the main nav bar.
 * @param enabled If false the hook is disabled
 */
export const useFullPage=(enabled=true)=>;

/**
 * Removes all margins and paddings from the main layout while keeping the main navigation and
 * other shared UI elements
 * @param enabled If false the hook is disabled
 */
export const useNoMargins=(enabled=true)=>;

/**
 * Returns true if the page should be displayed in full screen
 */
export const useIsInFullPageMode=():boolean=>{
    const count=useSubject(fullPageSubject);
    return count>0;
}

/**
 * Returns true if the page should remove all margins
 */
export const useIsNoMarginMode=():boolean=>;
```