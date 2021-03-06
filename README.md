## 5 Understanding the VueJS Instance

### -- Some Basics about the VueJS Instance

The VueJS instance is the middleman between the DOM (the HTML) and the business logic.
```js
    new Vue({
        el: '',
        data: {},
        methods: {},
        computed: {},
        watch: {}
    })
```

### -- Using Multiple Vue Instances

It is possible to control multiple pieces of the application with multiple instances.
Be aware, inside an instance you can only access the properties of that specific instance with the `this` keyword.  

### -- Accessing the Vue Instance from Outside

Store the Vue Instance in a variable, often used `vm` for "Vue Model".

It is recommended in case multiple instances need to communicate, to place all logic from into one instance. Since they are related in some way.

### -- How VueJS manages your Data and Methods

Behind the scens, when creating an instance, VueJS will take the passed object and then will take the data properties and methods and use them as native properties on the instance object itself.  
It even sets up a watcher for each of the properties which makes sure it recognizes whenever one of these properties is changed so it knows when to update the DOM or do anything.

The 'proxied' properties result in getter and setter properties in the rendered Vue Instance Object (the watcher-effect).

You can add new properties to the instance object, but they will not be watched by VueJS.

### -- A Closer Look at $el and $data

$el refers to the template (the HTML) of the instance.  
$data is an object which holds the data-properties.
You can also create the data variable before creating a Vue instance and simply pass it as the value for the key-value pair.

```js
var data = {
    ...
};

new Vue({
    data: data
})
```

### -- Placing $refs and Using them on your Templates

The `ref`-attribure is a key added by VueJS and not an official HTML-attribute. 
You can place it on any element. 
It isn't a directive so it doesn't need to bind or need a colon in the beginning.
```html
<button @click="show" ref="myButton">Show Paragraph</button>
```
```js
...
vm.$refs.myButton.innerText = 'Bladiebla'.
...
```

Keep in mind, this is not reactive. So the changes may be overwritten since it is directly the DOM and not part of the Vue instance.  
It is convenient to use a reference to get a value or access a native element. It is easier than a query-selector to gain access.

### -- Where to learn more about the Vue API

## [vuejs.org/api](http://vuejs.org/api)

### -- Mounting a Template

Mount is a method which does the same as the el-property.  
We can pass the element where to mount the application by passing the css-selector of the place to mount.

```js
    vm1.$mount('#app');
```

Up until now the template was already defined in the HTML.  
with the property 'template' you can create the html in the object.

```html
<div id="vm3"></div>
```
```js
var vm = new Vue({
    template: '<h1>Hello!</h1>'
});

vm.$mount('#app');
```

### -- Using Components

Create a Vue Component with `Vue.component()`. It takes as a first argument the selector of the component as a `string`. And as a second argument an object, similar to the Vue instance but not equal; for example "data" is used differently.

```js
Vue.component('hello', {
    template: '<h1>Hello!</h1>'
});
```

To use it: Which replaces the custom element with the template.
```html
<hello></hello>
```

### -- Limitations of some Templates

Template would be a `string` and is therefor harder to write multiline, or IDE support.

### -- How VueJS updates the DOM

Each property has it's own watcher.  
Accessing the real DOM is the part which takes the most performance.  
Vue add an exta layer: the Virtual DOM; a copy parsed in JavaScript and therefor real quickly to access.

### -- The VueJS Lifecycle

It start with the `new Vue()`-constructor,  
Then we access the first lifecycle method to which we can listen `beforeCreate()`.
Then it initialzes Data & Events and then creates the instance and call the `created()`
-hook-method.  
Thereafter it compiles the template or derifes the template from the HTML.  
Then `beforeMount()` is reached. This is called right before this template is mounted to the real DOM, so it is not there yet.  
Now the element is replaced with the compiled template, it is still not mounted but VueJS compiles the template, inserts all the values, sets up all bindings and converts it into real HTML-code BUT still behind the scenes.  
Then it is mounted to the DOM.

Ongoing lifecycle:
If some data changes, the DOM is rerendered.
The `beforeUpdate()`-hook-method right before the part of the DOM is rerendered. And the `updated()`-hook-method which is called right after the DOM is updated.

Before-destroy-lifecycle:
`beforeDestroy()` right before an instance is destroyed. This ends with the `destroyed()`-hook-method.

### -- The VueJS Instance Lifecycle in Practice

```js
new Vue({
    ...
    beforeCreate: function(){
        console.log('beforeCreate()');
    },
    created: function(){
        console.log('created()');
    },
    beforeMount: function(){
        console.log('beforeMount()');
    },
    mounted: function(){
        console.log('mounted()');
    },
    beforeUpdate: function(){
        console.log('beforeUpdate()');
    },
    updated: function(){
        console.log('updated()');
    },
    beforeDestroy: function(){
        console.log('beforeDestroy()');
    },
    destroyed: function(){
        console.log('destroyed()');  
    } 
}); 
```

--- 
---
## Section 6 - Moving to a "Real" Development Workflow with Webpack and Vue CLI

### -- Why do we need a Development Server?

- Test the App under realistic circumstances
- To lazy load files
- Benefits like auto-reload

### -- What does 'Development Workflow' mean?

//

### -- Using the Vue CLI to create Projects

Vue CLI allows you to fetch empty VueJS Project Templates

```
npm i -g vue-cli
```

Choose from different Templates:
- simple: index.html + Vue CDN import
- webpack-simple: Basic Webpack Workflow
- webpack: Complex Webpack Workflow (incl. Testing)
- browserify / browserify-simple: Browserify Workflows

Template compilation is supported in all options except for 'simple'.

### -- Installing the Vue CLI and Creating a new Project

Initialize a new project with vue cli:

```js
// vue init [template] [project-folder-name]
vue init webpack-simple vue-cli
```

### -- An Overview over the Webpack Template Folder Structure

The first file that gets executed is "main.js" when the bundle gets loaded in index.html.


### -- Understanding ".vue" Files

The Single Template File.

In main.js all thats happening is the creation of a new Vue instance which depends on the vue folder and to get access to create a new instance.
Then it selects the Vue element with the el-property, which indicates the place where to load the Vue-app.

Instead the `render`-property-function tells VueJS to take this place but don't infer the template, overwrite it with the html-template exported in App (App.vue).

```js
import Vue from 'vue';
import App from './App.vue'

new Vue({
    el: '#app',
    render: h => h(App)
});
```
Inside a .vue-file the markup follows the same structure:
1. A `<template></template>`-block; the container of the HTML
2. A script holding the VueJS code for this template.
3. And some possible styling.

*in App.vue:*
```html
    <template>
        <h1>Hello World!</h1>
    </template>

    <script>
        export default {

        }
    </script>
```

### -- Understanding the Object in the Vue File

Whatever is exported in the object is a Vue instance - the same object used by VueJS.

### -- More about ".vue" Files and the CLI

Learn more about '.vue' Files in this Article from the official Docs: http://vuejs.org/guide/single-file-components.html

Learn more about the `render()` method in another Article in the official Docs: http://vuejs.org/guide/render-function.html

Finally, it's important to be aware of the fact, that you can also load your App.vue File (your main Component/Instance) via the following two ways (Alternatives to `render()`);

**1) Using the ES6 Spread Operator:**  
Install the babel-preset-stage-2 Dependency to your .babelrc File)
```cmd
npm i -D babel-preset-stage-2
```
*in .babelrc:*
```json
    {
        "presets": [
            ["es2015", {"modules": false}],
            ["stage-2"]
        ]
    }
```

*in main.js:*
```js
    import Vue from 'vue'
    import App from './App.vue'

    new Vue({
        el: '#app',
        ...App
    });
```

**2) Using `mount()`:**  
Also install the stage-2 preset as described above.

*in main.js:*
```js
    import Vue from 'vue'
    import App from './App.vue'

    const vm = new Vue({
        ...App
    });

    vm.$mount('#app');
```

Learn more about the CLI here: https://github.com/vuejs/vue-cli


## Section 7 - Components

To create & use a component:

1. Define render-area.  
*in index.html:* 
```html
<div id="app"></div>
```
2. Create and Mount an Vue instance.  
*in main.js:*
```js
import Vue from 'vue'

const vm = new Vue({

});

vm.$mount('#app'); 
```
3. Create and pass the Single Template file.  
*in App.vue:*
```html
<template>
    <div>

    </div>
</template>
<script></script>
```
*in main.js:*
```js
//import Vue from 'vue'
import App from './App.vue'

//const vm = new Vue({
    ...App
//})
```
4. Create a Component.  
*in main.js:*
```js
//import Vue from 'vue'
//import App from './App.vue'

Vue.component('my-cmp', {
    
});

//const vm = new Vue({
    //...App
//})
```
5. Create & import the components' template  
*in App.vue:* 
```html
<!-- <template> -->
    <!-- <div> -->
        <my-cmp></my-cmp>
    <!-- </div> -->
<!-- </template> -->
```
*in MyComponent.vue:*
```html
<template>
  <p>Server Status: {{ status }}</p> 
</template>

<script>
export default {
  data () {
    return {
      status: 'Critical'
    }
  }
}
</script>
```

*in main.js:* 
```js
//import Vue from 'vue'
//import App from './App.vue'
import MyComponent from './MyComponent.vue'

//Vue.component('my-cmp', {
    ...MyComponent
//});

//const vm = new Vue({
    //...App
//})
```
### -- Storing Data in Components with the Data Method

If `data` isn't a function, the components will share the same value. Thus when one changes, they all change!
When it is a function, each component gets its own data storage.

### -- Registering Components Locally and Globally

*in main.js:*
```js
//-import MyComponent from './MyComponent.vue'-

// Registers component globally
//-Vue.component('my-cmp', {});-
```
*in MyComponent.vue `script`:*
```js
import MyComponent from './MyComponent.vue'

// Registers component locally
var cmp = {
    ...MyComponent
}

export default {
    components: {
        'my-cmp': cmp
    }
}
```

### -- The "Root Component" in the App.vue File

// 

### -- #95 Creating a Component

One of the restrictions of a Single Template File is   
`template should contain exactly one root element`.

The solution is to wrap all in a container `div`.

### -- #96 Using Components

*in main.js:*
```js
    import Vue from 'vue'
    import App from './App.vue'
    import Home from './Home.vue'

    //global component
    Vue.component('app-servers', Home);

    const vm = new Vue({
        ...App
    });

    vm.$mount('#app');
```
*in App.vue:*
```html 
<template>
    <app-servers></app-servers>
</template>
```
*in Home.vue:*
```html
<template>
    <div>
        <app-server-status v-for="server in 5"></app-server-status>
    </div>
</template>
<script>
    import ServerStatus from './ServerStatus.vue'

    export default {
        components: {
            'app-server-status': ServerStatus
        }
    }
</script>
```
*in ServerStatus.vue:*
```html
<template>
    <div>
        <p>Server status: {{ status }}</p>
        <hr>
        <button @click="changeStatus">Change Status</button>
    </div>
</template>
<script>
    export default {
        data: function(){
            return {
                status: 'Critical'
            }
        },
        methods: {
            changeStatus() {
                this.status = 'Normal'
            }
        }
    }
</script>
```
**! Add `:key="[iterator].id"` to resolve conflicts**

### -- #99 How to Name your Component Tags (Selectors)

```html
<template>
    <appHeader></appHeader> <!-- 1. -->
    <app-header></app-header> <!-- 2. -->
    <app-header></app-header> <!-- 3. -->
    <appheader></appheader> <!-- 4. -->
</template>
<script>
    import AppHeader from './AppHeader.vue'

    export default {
        compontents: {
            appHeader: AppHeader // 1. Case-sensitive Name
            'app-header': AppHeader // 2. Classical setup
            appHeader: AppHeader // 3. Combination 1 & 2, more JS-style
            AppHeader // 4. ES6 option, will automaticly create key-value pair "AppHeader: AppHeader"
        }
    }
</script>
```
Option 2 and 3 are considered as normal or even best practice.

### -- #100 Scoping Component Styles

By default the style-tag in a Single Template File is globally used.
To scope the styling, add the attribute `scoped` to the style-tag:
```html
<style scoped></style>
```

### -- #102 Module Resources & Useful Links

Learn more about VueJS Components: http://vuejs.org/guide/components.html

## Section 8 - Communicating between Components

### -- #105 Using Props for Parent => Child Communication

*in App.vue:*   
```html 
<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12">
                <app-user></app-user>
            </div>
        </div>
    </div>
</template>

<script>
    import User from './components/User.vue';

    export default {
        components: {
            appUser: User
        }
    }
</script>

<style>
    div.component {
        border: 1px solid black;
        padding: 30px;
    }
</style>
```
*in User.vue:*  
1: Add `<button>Change my Name</button>` to template html.  
2: Add `data`-function-property to vue instance.  
3: Return `name`-property in `data-function-property`.   
4: Add click-event `@click="changeName"` to `button`.     
9: Add the binded `:name`-key to `<app-user-detail>`.  
10: Pass the `name`-property to the `:name`-key.
```html
<template>
    <div class="component">
        <h1>The User Component</h1>
        <p>I'm an awesome User!</p>
        <button @click="changeName">Change my Name</button>
        <hr>
        <div class="row">
            <div class="col-xs-12 col-sm-6">
                <app-user-detail :name="name"></app-user-detail>
            </div>
            <div class="col-xs-12 col-sm-6">
                <app-user-edit></app-user-edit>
            </div>
        </div>
    </div>
</template>

<script>
    import UserDetail from './UserDetail.vue';
    import UserEdit from './UserEdit.vue';

    export default {
        data() {
            return {
                name: "Nick"
            }
        },
        methods: {
            changeName() {
                this.name = "Ronan the Accuser"
            }
        },
        components: {
            appUserDetail: UserDetail,
            appUserEdit: UserEdit
        }
    }
</script>

<style scoped>
    div {
        background-color: lightblue;
    }
</style>
```
*in UserDetail.vue:*  
6: Add display-area `<p>User Name: {{ name }}</p>` to template html.  
7: Add `props`-array-property to Vue instance.  
8: Add `name`-key to `props`-array.
```html
<template>
    <div class="component">
        <h3>You may view the User Details here</h3>
        <p>Many Details</p>
        <p>User Name: {{ name }}</p>
    </div>
</template>

<script>
    export default {
        props: ['name']
    }
</script>

<style scoped>
    div {
        background-color: lightcoral;
    }
</style>
```

### -- Using "props" in the Child Component

//

### -- #108 Validating "props"

The `props`-property can not only be an array. For validation it should be an object.

```js
props: {
    myName: String // forces the value to be of type string
    myName: [String, Array] // for multiple types
}
```
It is also possible to define the property as an object for extra options:
```js
props: {
    myName: {
        type: String,
        default: 'Nick'
        // required: true
    }
}
```
### -- #109 Using Custom Events for Child => Parent Communication

in *UserDetails.vue:*  
1: Add `<button>Reset Name</button>` to template html.  
2: Add click-event `@click="ResetName"` to `button`.  
3: Add `resetName`-function-property to Vue instance.  
4: Add `$emit`-method to custom function.  
5: Pass a custom event-name to `$emit`-method as a first argument.  
6: Pass data as a second argument.
```html
<template>
    <div class="component">
        <h3>You may view the User Details here</h3>
        <p>Many Details</p>
        <p>User Name: {{ name }}</p>
        <button @click="resetName">Reset Name</button>
    </div>
</template>

<script>
    export default {
        props: {
            name: {
                type: String,
                default: "Nick"
            }
        },
        // data() {
        //     return {
        //         userName: this.name
        //     };
        // },
        computed: {
            output() {
                return {
                    userName: this.name
                }
            }    
        },
        methods: {
            resetName() {
                this.userName = "Amnesia";
                this.$emit('nameWasReset', this.userName);
            }
        }
    }
</script>
```

*in parent User.vue:*
7: Add the listener `@[customEventName]=` to the component emitting the event.
8: Pass the listener the code to be executed, like a method or the property-change (`"name = $event"`).
```html
<template>
    <div class="component">
        <h1>The User Component</h1>
        <p>I'm an awesome User!</p>
        <button @click="changeName">Change my Name</button>
        <hr>
        <div class="row">
            <div class="col-xs-12 col-sm-6">
                <app-user-detail :name="name" @nameWasReset="name = $event"></app-user-detail>
            </div>
            <div class="col-xs-12 col-sm-6">
                <app-user-edit></app-user-edit>
            </div>
        </div>
    </div>
</template>

<script>
    import UserDetail from './UserDetail.vue';
    import UserEdit from './UserEdit.vue';

    export default {
        data() {
            return {
                name: "Nick"
            }
        },
        methods: {
            changeName() {
                this.name = "van der Sangen";
            }
        },
        components: {
            appUserDetail: UserDetail,
            appUserEdit: UserEdit
        }
    }
</script>
```

#### ! Use either the `data`- or `computed`-property to resolve conflicts in mutating !

### -- #110 Understanding Unidirectional Data Flow

//

### -- #111 Communicating with Callback Functions

Alternitive for using Custom Events it is possible to use Callback functions.

in *UserDetails.vue:*  
1: Add `<button>Reset Name</button>` to template html.  
2: Add click-event `@click="resetFn"`-prop to `button`.  
3: Add `resetFn`-prop, of type `Function`, to `props`-object in Vue instance.
```html
<template>
    <div class="component">
        <h3>You may view the User Details here</h3>
        <p>Many Details</p>
        <p>User Name: {{ name }}</p>
        <button @click="resetFn">Reset Name</button>
    </div>
</template>

<script>
    export default {
        props: {
            name: {
                type: String,
                default: "Nick"
            },
            resetFn: {
                type: Function
            }
        }
    }
</script>
```

*in parent User.vue:*  
4: Add `resetName`-function-property to `methods`-object in Vue instance. 
5: Add the binded `:resetFn`-key to `<app-user-detail>`.  
6: Pass the `resetName`-property to the `:resetFn`-key.
```html
<template>
    <div class="component">
        <h1>The User Component</h1>
        <p>I'm an awesome User!</p>
        <button @click="changeName">Change my Name</button>
        <hr>
        <div class="row">
            <div class="col-xs-12 col-sm-6">
                <app-user-detail :name="name" :resetFn="resetName"></app-user-detail>
            </div>
            <div class="col-xs-12 col-sm-6">
                <app-user-edit></app-user-edit>
            </div>
        </div>
    </div>
</template>

<script>
    import UserDetail from './UserDetail.vue';
    import UserEdit from './UserEdit.vue';

    export default {
        data() {
            return {
                name: "Nick"
            }
        },
        methods: {
            changeName() {
                this.name = "van der Sangen";
            },
            resetName() {
                this.name = "Nick";
            }
        },
        components: {
            appUserDetail: UserDetail,
            appUserEdit: UserEdit
        }
    }
</script>
```

### -- #112 Communication between Sibling Components

*in User.vue:*
```html
<template>
    <div class="component">
        <div class="row">
            <div class="col-xs-12 col-sm-6">
                <app-user-detail :userAge="age"></app-user-detail>
            </div>
            <div class="col-xs-12 col-sm-6">
                <app-user-edit :userAge="age"></app-user-edit>
            </div>
        </div>
    </div>
</template>

<script>
    import UserDetail from './UserDetail.vue';
    import UserEdit from './UserEdit.vue';

    export default {
        data() {
            return {
                age: 29
            }
        },
        components: {
            appUserDetail: UserDetail,
            appUserEdit: UserEdit
        }
    }
</script>
```
*in UserDetail.vue:*
```html 
<template>
    <div class="component">
        <p>User Age: {{ userAge }}</p>
    </div>
</template>

<script>
    export default {
        props: {
            userAge: {
                type: Number
            }
        }
    }
</script>
```
*in UserEdit.vue:*
```html
<template>
    <div class="component">
        <p>User Age: {{ userAge }}</p>
        <button @click="editAge">Edit Age</button>
    </div>
</template>

<script>
    export default {
        props: {
            userAge: {
                type: Number
            }
        },
        methods: {
            editAge() {
                this.userAge = 30;
            }
        }
    }
</script>
```
#### Method #1: Setup Custom Event to be emitted
*in UserEdit.vue:*  
1: Call `$emit` in the `editAge`-function.
```js
//methods: {
    //editAge(){
        //this.userAge = 30;
        this.$emit('ageWasEdited', this.userAge);
    //}
//}
```
*in User.vue:*  
2: Setup listener in `<app-user-edit>`-component.
```html
    <app-user-edit @ageWasEdited="age = $event"></app-user-edit>
```
*in UserEdit.vue:*  
3: Resolve mutations bij referring to the passed prop with `data` or `computed`.
```js
//methods: {
    //editAge(){
        //---this.userAge = 30;--- 
        //---this.$emit('ageWasEdited', this.userAge);---
        this.age = this.age
        this.$emit('ageWasEdited', this.age);
    //}
//},
data() {
    return {
        age: this.userAge
    };
},
computed: {
    output() {
        return {
            age: this.userAge
        }
    }    
},
```
#### Method #2: Callback Function
*in UserEdit.vue:*  
1: 1: Add click-event `@click="editAgeFn"`-prop to `button`.  
2: Add `editAgeFn`-prop to `props`-object in Vue instance.
```html
<button @click="editAgeFn">Edit Age</button>
```

```js
 export default {
        props: {
            userAge: {
                type: Number
            },
            editAgeFn: Function
        }
    }
```

*in User.vue:*  
3: Add `:editAgeFn`-key to `<app-user-edit>`.  
4: Pass the `editAge`-property to the `:editAgeFn`-key.
5: Add `editAge`-function to `methods`-object in Vue instance.
```html
<app-user-edit :userAge="age" :editAgeFn="editAge">
```
```js
 methods: {
    editAge() {
        this.age = 30;
    }
},
```
#### Method #3: Event Bus

### -- #113 Using an Event Bus for Communication

*in main.js:*  
1: Create a new Vue instance. **Before mounting**
```js
export const eventBus = new Vue();
```
*in UserEdit.vue:*  
2: Import the new Vue instance.  
3: Call `$emit` on the new Vue instance. 
4: Pass the Custom Event & to-emitted-data. 
```js
import { eventBus } from '../main';

export default {
    props: ['userAge'],
    methods: {
        editAge() {
            this.userAge = 92;
            eventBus.$emit("ageWasEdited", this.userAge);
        }
    }
}
```
*in UserDetail.vue:*  
5: Import the new Vue instance aswell.  
6: Add the `created`-lifecycle-hook-function to the main Vue instance.  
7: Setup `$on`-listener to the Custom Event on the new Vue instance and a callback function. Always pass the data.  
```js
import { eventBus } from '../main';

export default {
    props: ['userAge'],
    created() {
        eventBus.$on('ageWasEdited', (emit) => {
            this.userAge = emit;
        })
    }
}
```

*in both UserDetails.vue & UserEdit.vue:*  
8: Avoid mutating props directly by referring to it in the `data`-function.  
9: Update all other references to `userAge` with `age`.
```js
    data() {
        return {
            age: this.userAge
        }
    },
```

This is managing state of properties across multiple components can very quick be very complicated. To make this simpler VueJS has a great tool called "VueX".

### -- #114 Centralizing Code in an Event Bus

Another way of managing the data is to set the eventBus-instance up as a collection of data & methods.  
It can be any code, accessible from anywhere in your application as long as it is imported and accesses the provided methods on the instance.

*in main.js:*  
1: Create a eventBus-vue-instance.  
2: Add the `data`- & `methods`-object.  
3: Add the `changeAge`-function to the `methods`-object and pass it an argument to which the age can be passed in.  
4: Call the `$emit`-function and pass it the same custom event & the passed argument.
```js
export const eventBus = new Vue({
    data: {

    }, 
    methods: {
        changeAge(newAge){
            this.$emit("ageWasEdited", newAge);
        }
    }
});
```
*in UserEdit.vue:*  
5: Call the `changeAge`-function on the eventBus-instance and pass it the `userAge`-prop.
```js
    methods: {
        editAge(){
            this.userAge = 30;
            //---eventBus.$emit('ageWasEdited', this.userAge);---
            eventBus.changeAge(this.userAge);
        }
    }
``` 
### // Exercise 7

//

### -- Module Resources & Useful Links

- Official Docs - Props: http://vuejs.org/guide/components.html#Props
- Official Docs - Custom Events: http://vuejs.org/guide/components.html#Custom-Events
- Official Docs - Non-Parent-Child Communication: http://vuejs.org/guide/components.html#Non-Parent-Child-Communication

---
---

## Section 9 - Advanced Component Usage

### -- #118 Setting up the Module Project

*in index.html*
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Vue Components</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
  </head>
  <body>

    <div id="app"></div>

    <script src="/dist/build.js"></script>
  </body>
</html>
``` 
*in main.js:*
```js 
import Vue from 'vue'
import App from './App.vue'

const vm = new Vue({ 
  ...App
});

vm.$mount('#app');    
```
*in App.vue:*
```html
<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12">
                <quote></quote>
            </div>
        </div>
    </div>
</template>
<script>
    import Quote from './components/Quote.vue'

    export default {
        components: {
            Quote
        }
    }
</script>
```
*in Quote.vue:*
```html
<template>
    <div>
        <p>A wonderful Quote!</p>
    </div>
</template>
<script></script>
<style scoped>
    div {
        border: 1px solid #ccc;
        box-shadow: 1px 1px 2px black;
        padding: 30px;
        margin: 30px auto;
        text-align: center;
    }
</style>
```

### -- #119 Passing Content - The Suboptimal Solution

**Using Props:**

*in App.vue:*  
1: Add the attribute `quote` to the `<quote>`-component.  
2: Pass it the quote as a string.
```html
<quote quote="A wonderful Quote!"></quote>
```

*in Quote.vue:*  
3: Add the `quote`-prop to the `quote`-object in the Vue instance.  
4: Express the value of `{{ quote }}` in the template-html.
```html
<p>{{ quote }}</p>

<script>
    export default {
        props: {
            quote: {
                type: String
            }
        }
    }
</script>
```

### -- #120 Passing Content with Slots

*in Quote.vue:*    
1: Add the reserved word **slot** in the template-html.  
```html
<template>
    <div>
        <slot></slot>
    </div>
</template>
```
*in App.vue:*  
2: Contain the html to be injected in the template within the `<quote>`-component.
```html
<quote>
    <h2>The Quote</h2>
    <p>A wonderful Quote</p>
</quote>
```

### -- #121 How Slot Content gets Compiled and Styled

Add styling to the child-component where the html is rendered.  
Everything else is handled on the parent-component.

### -- #122 Using Multiple Slots (Named Slots)

Split the HTML by adding names to the slot's content with the `slot`-attribute.

*in App.vue:*
```html
<quote>
    <h2 slot="title">{{ quoteTitle }}</h2>
    <p slot="content">A wonderful Quote!</p>
</quote>
```
*in Quote.vue:*
```html
<div class="title">
    <slot name="title"></slot>
</div>
<hr>
<div>
    <slot name="content"></slot>
</div>
```
### -- #123 Default Slots and Slot Defaults

VueJs will treat the unnamed `<slot>` as the "default"-slot: Everything that is passed in which doesn't have a named-slot assigned, will automaticly be rendered in the default slot.

### -- #124 A Summary on Slots

//

### -- #125 Switching Multiple Components with Dynamic Components

// 

### -- #126 Understanding Dynamic Component Behavior

On switching between components the current component gets destroyed and a new one is created.

### -- #127 Keeping Dynamic Components Alive

Wrap the reserved tag `<component>` in another reserved tag `<keep-alive>` to preserve the state.

### -- #128 Dynamic Component Lifecycle Hooks

Two new lifecycle hooks:

```js
    deactivated() {},
    activated() {}
```

### // Exercise 8: Slots and Dynamic Components

//

### -- #130 Module Resources & Helpful Links

- Official Docs - Slots: http://vuejs.org/guide/components.html#Content-Distribution-with-Slots
- Official Docs - Dynamic Components: http://vuejs.org/guide/components.html#Dynamic-Components
- Offical Docs - Misc: http://vuejs.org/guide/components.html#Misc

## // Project: Wonderful Quotes

## Section 11 - Handling User Input with Forms

### -- A Basic input Form Binding
*in App.vue:*
```html
<template>
    <div class="form-group">
        <label for="email">Mail</label>
        <input type="text" id="email" class="form-control" v-model="user.email">
    </div>

    <div class="panel-body">
        <p>Mail: {{ user.email }}</p>
    </div>
</template>

<script>
    export default {
      data () {
        return {
          user: {
            email: ''
          }
        }
      }
    }
</script>
```

VueJS checks what the source is of the editting and automatically update the corresponding property.

### -- #146 Modifying User Input with Input Modifiers

By default the input-field gets updated at every input.  
By adding the modifier `lazy` to the `v-model` it gets update at every change.

```html
<input type=text v-model.lazy="userData.password" />
```

- `trim` to trim any whitespace in the beginning and end.
- `number` to force conversion to number instead of string.

### -- #147 Binding textarea and Saving Line Breaks

To preserve line breaks in the `<textarea>`, add the CSS-property `white-space: pre` to the output element to keep the multiline string.

```html
<textarea v-model="message"></textarea>

<p style="white-space: pre">{{ message }}</p>
```
### -- #148 Using Checkboxes and Saving Data in Arrays

1: Create a `data`-property which is an array.  
2: Add the `v-model` to the checkbox' input-fields.
3: Loop through the array and show each item.
```html
<input type="chekbox" value="Promotions" v-model="sendMail"/>
<input type="chekbox" value="News" v-model="sendMail"/>

<ul>
    <li v-for="mail in sendMail" :key="mail.id">{{ mail }}</li>
</ul>

<script>
    data () {
        sendMail: []
    }
</script>
```