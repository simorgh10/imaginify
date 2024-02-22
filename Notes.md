
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

# Clerk Authentication
* most comprehensive user management platform, sign-in-box but also complete suite of embeddable UIs, flexible APIs, and admin dashboards to authenticate and manage your users
* name JSM_Imaginify, Email, Google, Github -> create app > copy env variables into .env.local
* In clerk config, email > enable UserName
* Home n> continue in docs

> npm install @clerk/nextjs

* wrap app with a clerk provider in base layout

* middleware.ts file within src dir. keep it as it is in doc

* now going to localhost:3000 will redirect to clerk authentication

* auth pages are not within our routes. We would like to create those pages within our route structure so that windows appears directly within our routing structure
* (auth)/sign-up/[[...sign-up]]/page.tsx
* same structure for sign-in
* in .env.local: NEXT_PUBLIC_CLERK_SIGN_IN_URL (/sign-in), NEXT_PUBLIC_CLERK_SIGN_UP_URL (/sign-up), NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL (/), NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL (/)
* then step 5 from documentation

* then customize auth page -> Branding, set Application Name and upload logo and icon
* set the appearance of ClerkProvider
* Note that auth component is centered thanks to user defined auth class (see globals.css) that uses uer defined flex-center class

# Layout sidebar & Mobile navigation
* components/shared
* src/constants/index.ts
* get t from README
* usePathname hook coming from next/navigation
* add shadcn button

> npx shadcn-ui@latest add button

* for mobile nav install another shadcn component called a sheet
> npx shadcn-ui@latest add sheet
* Extends the dialog component to display content that complements the main content of the screen


