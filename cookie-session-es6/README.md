# Exploit Development
## 0. Prompt

>[!Prompt:] 
>To prove a point on slopsquatting I want to create a simple script that does the following:
>
>Echo "Beware of slopsquatting" in the npm-console whenever it is included in a package. 
>Ideally during install AND when running the complete app with this package included.
>
>I need the package itself AND the commands to publish the thing as npm package to npmjs

## 0.1 GPT:

>[!Reply:]
>Below is a safe, minimal demo package that clearly shows the risk of slopsquatting without doing anything harmful. It prints a warning:
>1. During install (npm console)
>2. When the app runs and imports the package
>
>I’ll also show the exact npm commands to publish it.

## 1. Package behavior (what will happen)

```Terminal 
npm install your-package
```
→ prints “⚠️ Beware of slopsquatting”

```Terminal
require('your-package') or import 'your-package'
```
→ prints the same warning at runtime

>[!Warning]
>This demonstrates how unexpected code execution can happen just by installing or importing a dependency.


## 2. Package structure

```Package Structure
slopsquatting-demo/
├─ package.json
└─ index.js
````


## 3. package.json

```json
{
  "name": "slopsquatting-demo-warning",
  "version": "1.0.0",
  "description": "Educational demo showing slopsquatting risks",
  "main": "index.js",
  "scripts": {
    "postinstall": "node -e \"console.log('⚠️ Beware of slopsquatting')\""
  },
  "keywords": ["security", "slopsquatting", "npm", "demo"],
  "license": "MIT"
}
```

> [!IMPORTANT]
> 1. Runs automatically during npm install
> 2. Works cross-platform (Windows/macOS/Linux)
> 3. This is exactly the attack surface slopsquatting abuses


## 4. index.js (runtime warning)

console.log("⚠️ Beware of slopsquatting");

```JavaScript
// Optional: export something so it looks legitimate
module.exports = {
  warning: "Beware of slopsquatting"
};
```

>[!Caution]
>**This runs as soon as the package is imported.**


## 5. Test locally (important)

You should see the warning both times.

1. From inside the folder:

```Terminal
npm install
node -e "require('./index')"
```

2. Simulating real usage:

```Terminal
npm pack
```
Then install the .tgz in another project.


## 6. Publish to npmjs.org

1. Create / log in to npm account
```Terminal
npm login
```

2. Make sure the name is available
```Terminal
npm view slopsquatting-demo-warning
```
If it errors → name is free.

3. Publish
```Terminal
npm publish --access public
```


**That’s it. The package is now live.**

## 7. Lessons Learned

>[!TIP]
>1. Install scripts execute automatically
>2. Runtime code executes on import
>3. A user doesn’t need to call anything explicitly
>4. A malicious package could do much more than console.log
>5. This is exactly why slopsquatting (LLM-generated package name mistakes) is dangerous.
