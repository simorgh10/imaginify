
* https://www.youtube.com/watch?v=Ahwoks_dawU

# Setup

> npx create-next-app@latest

choose defaulsts

* install shadcn
> npx shadcn-ui@latest init

Default, Slate, Yes

> npm run dev

* Adapt metadata, change font, use antialiased (css property that makes some fonts easier to read)

* from https://github.com/adrianhajdin/ai_saas_app/blob/main/README.md get tailwind config snippet and paste it locally

* the same for globals.css

* the same for public folders (that contains images). Download it. Unzip it. And use it a a public folder for the project. move favicon.ico into app folder

* Try out Arc navigator

# Next.js Routing & folder structure
* Route Group to prevent the folder from being included in the route's URL path. allows you yo organize your route segments and project fiuoesinto logical groups without affecting the URL path structure

* (auth) route group

* (root) layout move initially generated page.tsx to that route group. The initially generated layout stays where it is.

* use dynamic routes for transformations

* file based routing, route groups and creeating different layouts for each one of these route groups

* 