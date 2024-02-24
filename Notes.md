
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

# Database & Models Setup
* MongoDB Atlas
* signup, create deployment, choose user and password, go to network access > add IP access List ENtry > Allow access from anywhere > confirm, then overview, connect > select drivers > get nstall command and connection string

> npm install mongodb mongoose

* also installed mongoose

* add MONGODB_URL to the .env.local

* add lib > database > mongoose.ts

* In Next js, you connect to the databse on every request or server action,; because nextj apps run in a serverless environment. serverless functions are stateless meaning they start up to handle requests then shut down after.

* ENsures that each request is handled independently, allowing for better scalability, and reliability, as there is no need to manage persistent connections axcross many instances which works well with scalable and flexble nature of mongodb.
* But doing that means too many mongodb connections open for each and every action we perform on server side
* so we'll resoirt to caching our connections

* to create a model image.model.ts

* how NextJS caches things if execution is serverless ?
* || vs ??

* used ChatGPT to create interface for frontend
```
Create an IImage interface based off of the following ImageSchema:

const ImageSchema = new Schema({
  title: { type: String, required: true },
  transformationType: { type: String, required: true },
  publicId: { type: String, required: true },
  secureURL: { type: URL, required: true },
  width: { type: Number },
  height: { type: Number },
  config: { type: Object },
  transformationUrl: { type: URL },
  aspectRatio: { type: String },
  color: { type: String },
  prompt: { type: String },
  author: { type: Schema.Types.ObjectId, ref: "User" },
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now },
});
```

* Same for User, and Transaction models that you can copy from README snippets

* transaction created as additional reference between user and Image, because we have to keep track of the credits.

# Server Actions & Webhook

* first functionality: creating, updating & deleting users
* lib/actions/uer.actions.ts
* copy from README
* Unlike what I've seen in other projects, here actions.ts is noit created in routes folders but in lib folder

* override the complete utils.ts file from README
* Get src/types/index.d.ts from README snippets

> npm install qs

* use webhooks to sync data between clerk users and backend mongo user:
  + Clerk will trigger an event once a user signs up with a new clerk account then it will make a request with a payload containing all of that juicy clerk user data (firstname, lastname, hashedpassword, ...)
  + then will send that data over to event porocessing directly to our database so that we can create a new user within our database and sink those users up
* thus, connect clerk with webhooks to our mongodb database 
* the easiest way will require us to immediately deploy our app

* after committing
```
git branch -M main
git remote add origin https://github.com/simorgh10/imaginify.git
git push -u origin main
```
* then proceed with all the steps to deploy on Vercel
* we need the url of the deployed app
https://papaye-imaginify.vercel.app/

* go to doc page for syncing clerk to backend data with webhooks https://clerk.com/docs/users/sync-data#enable-webhooks
  1. got to clerk > webhooks > add endpoint > enter the endpoint url [vercel app url]/api/webhooks/clerk. In Message filtering section check user checkboxes
  2. Also copy the signing secret into .env.local WEBHOOK_SECRET
  3. unterstand the payload, 
  4. install the svix package (provides package for verifying the webhook signature)
    > npm install svix
  5. create endpoint in your app. Copy it from README snippets into app/api/webhooks/clerk/route.ts
  6. Add endpoint to middleware. Add /api/webhooks/clerk to publicRoutes

* Test webhooks. Commit & push to github code then deploy. Vercel automatically takes notes and start building it. We have to make sure the new env variable gets added to the env variables as well. Then redeploy your latest commit.

* On localhost, sign up woth a new user you haven't used before. Clerk this time lets our app know, hey this tiume a new uer is created, please add it to your db as well.
* WORKED !!! Database > Cluster0 > collections > users a user has been created. I checked also that the mongodb user docuùent _id has been added to clerk user public metadata (see route.ts. Still don't know what is the need for that.)

# Add Image Form

* now we'll create the add image form which allows our uer to add a new image transformations
* In Add TransformationTypePage, ...
* components/shared/Header.tsx

* components/shared/TransformationForm.tsx
* build with react-hook-form and Zod
* Form as wrapper, FormField, and zod for validation

* https://ui.shadcn.com/docs/components/form
> npx shadcn-ui@latest add form
* zod comes with shadcn form
* create form schema, import zod resolver and useform and then define the form and the submit handler
* after that, we can build outer form, 
> npx shadcn-ui@latest add input
* render out the entire form


* CustomField

* select

> npx shadcn-ui@latest add select

* New React knowledge: A set... from ueState can take a function ((prevState: S) => S) as argument

# Cloudinary Media Uploader
* https://console.cloudinary.com/

> npm install next-cloudinary

* get keys, CLOUDINARY_API_KEY, CLOUDINARY_API_SECRET, NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME
* settings > Upload > Enable Unsigned uploading then Add upload Preset
* name jsm_imaginify, folder imaginify, 
* Media Analysis and AI, turn on Enable Google Auto Tagging and set to somewhere around 0,5
* Explore, Add ons, Cloud.. AI Background removal free, Google Auto Tagging free

* We'll use 2 features. Cloudinary upload widgets + cloudinary image.
* shared/MediaUploader.tsx

> npx shadcn-ui@latest add toast

* CldUploadWidget
* What's this ?
```
import { PlaceholderValue } from "next/dist/shared/lib/get-img-props";
```


# Transformed Image Components
* 

# Image Server Action
* image.action.ts server actions add/update/delete/getById

* InsufficientCreditsModal

> npx shadcn-ui@latest add alert-dialog