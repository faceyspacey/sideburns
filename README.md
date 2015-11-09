# Sideburns
> **Sideburns** is a [Meteor](http://meteor.com) package which give you templates for React in a familiar [Blaze API](https://www.meteor.com/blaze) (giving you **helpers**, **events**, **onRendered**, **onCreated** etc) with a subset of [Spacebars](https://github.com/meteor/meteor/blob/devel/packages/spacebars/README.md) (aka Meteor flavored Handlebars).

**Why React?** – Well it gives us faster pageloads, SEO without Spiderable, accessibility for users without JavaScript and general improvements in page rendering speed.

**Why a Blaze/Spacebars API?** – Well it's way easier to learn and get started with, can't argue against that right (see examples below)?


## Installation

```bash
meteor add timbrandin:sideburns
```

## Demo

* http://jsx-templating.meteor.com/ (https://github.com/timbrandin/meteor-iosmorphic-react-templating)
* http://spacetalkapp.com (https://github.com/SpaceTalk/SpaceTalk-Homepage)
* http://timbrandin.com (https://github.com/timbrandin/resumeteor)

## Getting started

#### Ideas for version 0.3 (Using the new Meteor toolchain)

##### Disassembled

```jsx
// page.jsx
<template name="page">
  <div class="home">
    <ul>
      {{#each item in items}}
      <li class={{isSelected}}>{{item.name}}</li>
      {{(/each}}
    </ul>
  </div>
</template>

Template.page.helpers({
  isSelected(context) {
    return this.state.selected == context._id ? return 'selected' : '';
  }
});

Template.page.events({
  'click li': function(context) {
    this.setState({selected: context._id});
  }
});

class Page extends React.Component {
  getInitialState: function() {
    return {selected: false};
  }
  render() {
    return Template.page.template;
  }
});
```

##### Inline

```jsx
// page.jsx
class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {selected: false};
  }
  helpers: {
    isSelected(context) {
      return this.state.selected == context._id ? 'selected' : '';
    }
  }
  events: {
    'click li': function(context) {
      this.setState({selected: context._id});
    }
  }
  render() {
    return (<div class="home">
      <ul>
        {{#each item in items}}
        <li class={{isSelected}}>{{item.name}}</li>
        {{(/each}}
      </ul>
    </div>);
  }
}
```


### Simple component

<table width="100%"><thead><tr><th width="50%">Sideburns (.html.jsx)</th><th width="50%">React comparison</th></tr></thead><tbody><tr><td valign="top"><pre lang="jsx"><code>

<span class="pl-k rich-diff-level-one">&lt;</span>template name<span class="pl-k rich-diff-level-one">=</span><span class="pl-s rich-diff-level-one"><span class="pl-pds">"</span>Page<span class="pl-pds">"</span></span><span class="pl-k rich-diff-level-one">&gt;</span>
  <span class="pl-k rich-diff-level-one">&lt;</span>div <span class="pl-k rich-diff-level-one">class</span><span class="pl-k rich-diff-level-one">=</span><span class="pl-s rich-diff-level-one"><span class="pl-pds">"</span>page<span class="pl-pds">"</span></span><span class="pl-k rich-diff-level-one">&gt;</span>
    Hello world
  <span class="pl-k rich-diff-level-one">&lt;</span>/div<span class="pl-k rich-diff-level-one">&gt;</span>
<span class="pl-k rich-diff-level-one">&lt;</span>/template<span class="pl-k rich-diff-level-one">&gt;

</code></span></pre></td><td valign="top"><pre lang="jsx" class="vicinity rich-diff-level-zero"><code>

Page <span class="pl-k rich-diff-level-one">=</span> React.createClass({
  <span class="pl-en rich-diff-level-one">render</span><span class="pl-k rich-diff-level-one">:</span> <span class="pl-k rich-diff-level-one">function</span>() {
    <span class="pl-k rich-diff-level-one">return</span> (<span class="pl-k rich-diff-level-one">&lt;</span>div className<span class="pl-k rich-diff-level-one">=</span><span class="pl-s rich-diff-level-one"><span class="pl-pds">"</span>page<span class="pl-pds">"</span></span><span class="pl-k rich-diff-level-one">&gt;</span>
      Hello world
    <span class="pl-k rich-diff-level-one">&lt;</span>/div<span class="pl-k rich-diff-level-one">&gt;</span>);
  }
});

</code></pre></td></tr></tbody></table>

<!--
```jsx
<template name="Page">
  <div class="page">
    Hello world
  </div>
</template>
```
-->

<!--
```jsx
Page = React.createClass({
  render: function() {
    return (<div className="page">
      Hello world
    </div>);
  }
});
```
-->

### Component with reactivity

<table width="100%"><thead><tr><th width="50%">Sideburns (.html.jsx)</th><th width="50%">React comparison</th></tr></thead><tbody><tr><td valign="top"><pre lang="jsx"><code>

<pre class="vicinity rich-diff-level-zero">
<span class="pl-k rich-diff-level-one">&lt;</span>template name<span class="pl-k rich-diff-level-one">=</span><span class="pl-s rich-diff-level-one"><span class="pl-pds">"</span>Page<span class="pl-pds">"</span></span><span class="pl-k rich-diff-level-one">&gt;</span>
  <span class="pl-k rich-diff-level-one">&lt;</span>div <span class="pl-k rich-diff-level-one">class</span><span class="pl-k rich-diff-level-one">=</span><span class="pl-s rich-diff-level-one"><span class="pl-pds">"</span>page<span class="pl-pds">"</span></span><span class="pl-k rich-diff-level-one">&gt;</span>
    Hello {{name}}
  <span class="pl-k rich-diff-level-one">&lt;</span>/div<span class="pl-k rich-diff-level-one">&gt;</span>
<span class="pl-k rich-diff-level-one">&lt;</span>/template<span class="pl-k rich-diff-level-one">&gt;</span>

Template.Page.onCreated(<span class="pl-k rich-diff-level-one">function</span>() {
  <span class="pl-v rich-diff-level-one">this</span>.<span class="pl-c1 rich-diff-level-one">name</span> <span class="pl-k rich-diff-level-one">=</span> <span class="pl-k rich-diff-level-one">new</span> <span class="pl-en rich-diff-level-one">ReactiveVar</span>(<span class="pl-s rich-diff-level-one"><span class="pl-pds">'</span>React<span class="pl-pds">'</span></span>);
});

Template.Page.helpers({
  <span class="pl-en rich-diff-level-one">name</span>() {
    <span class="pl-k rich-diff-level-one">return</span> <span class="pl-v rich-diff-level-one">this</span>.<span class="pl-c1 rich-diff-level-one">name</span>.get();
  }
});

Template.Page.onRendered(<span class="pl-k rich-diff-level-one">function</span>() {
  <span class="pl-c1 rich-diff-level-one">setTimeout</span>(()<span class="pl-k rich-diff-level-one"> =&gt;</span> {
    <span class="pl-v rich-diff-level-one">this</span>.<span class="pl-c1 rich-diff-level-one">name</span>.set(<span class="pl-s rich-diff-level-one"><span class="pl-pds">'</span>React: With a Blaze API<span class="pl-pds">'</span></span>);
  }, <span class="pl-c1 rich-diff-level-one">2000</span>);
});</pre>

</code></span></pre></td><td valign="top"><pre lang="jsx" class="vicinity rich-diff-level-zero"><code>

<pre>Page <span class="pl-k">=</span> React.createClass({
  <span class="pl-en">getIntialState</span><span class="pl-k">:</span> <span class="pl-k">function</span>() {
    <span class="pl-k">return</span> {name<span class="pl-k">:</span> <span class="pl-s"><span class="pl-pds">'</span>React<span class="pl-pds">'</span></span>};
  },
  <span class="pl-en">componentDidMount</span><span class="pl-k">:</span> <span class="pl-k">function</span>() {
    <span class="pl-c1">setTimeout</span>(()<span class="pl-k"> =&gt;</span> {
      <span class="pl-v">this</span>.setState({name<span class="pl-k">:</span> <span class="pl-s"><span class="pl-pds">'</span>React: With a Blaze API<span class="pl-pds">'</span></span>});
    }, <span class="pl-c1">2000</span>);
  },
  <span class="pl-en">render</span><span class="pl-k">:</span> <span class="pl-k">function</span>() {
    <span class="pl-k">return</span> (<span class="pl-k">&lt;</span>div className<span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>page<span class="pl-pds">"</span></span><span class="pl-k">&gt;</span>
      Hello {<span class="pl-v">this</span>.state.<span class="pl-c1">name</span>}
    <span class="pl-k">&lt;</span>/div<span class="pl-k">&gt;</span>);
  }
});</pre>

</code></pre></td></tr></tbody></table>

<!--
```jsx
// {{name}} is parsed into {this.data.name}.
<template name="Page">
  <div class="page">
    Hello {{name}}
  </div>
</template>

Template.Page.onCreated(function() {
  this.name = new ReactiveVar('React');
});

Template.Page.helpers({
  name() {
    return this.name.get();
  }
});

// Same as onComponentDidMount.
Template.Page.onRendered(function() {
  setTimeout(() => {
    this.name.set('React: With a Blaze API');
  }, 2000);
});
```

```jsx
Page = React.createClass({
  getIntialState: function() {
    return {name: 'React'};
  },
  componentDidMount: function() {
    setTimeout(() => {
      this.setState({name: 'React: With a Blaze API'});
    }, 2000);
  },
  render: function() {
    return (<div className="page">
      Hello {this.state.name}
    </div>);
  }
});
```
-->

### Component with click handler

<table width="100%"><thead><tr><th width="50%">Sideburns (.html.jsx)</th><th width="50%">React comparison</th></tr></thead><tbody><tr><td valign="top"><pre lang="jsx"><code>

<pre><span class="pl-k">&lt;</span>template name<span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>Page<span class="pl-pds">"</span></span><span class="pl-k">&gt;</span>
  <span class="pl-k">&lt;</span>div <span class="pl-k">class</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>page<span class="pl-pds">"</span></span><span class="pl-k">&gt;</span>
    Hello world
  <span class="pl-k">&lt;</span>/div<span class="pl-k">&gt;</span>
<span class="pl-k">&lt;</span>/template<span class="pl-k">&gt;</span>

Template.Page.events({
  <span class="pl-s"><span class="pl-pds">'</span><span class="pl-en">click .page</span><span class="pl-pds">'</span></span><span class="pl-k">:</span> <span class="pl-k">function</span>() {
    <span class="pl-en">console</span><span class="pl-c1">.log</span>(<span class="pl-s"><span class="pl-pds">'</span>Hello world<span class="pl-pds">'</span></span>);
  }
});</pre>

</code></span></pre></td><td valign="top"><pre lang="jsx" class="vicinity rich-diff-level-zero"><code>

<pre>Page <span class="pl-k">=</span> React.createClass({
  <span class="pl-en">clickEvent</span><span class="pl-k">:</span> <span class="pl-k">function</span>() {
    <span class="pl-en">console</span><span class="pl-c1">.log</span>(<span class="pl-s"><span class="pl-pds">'</span>Hello world<span class="pl-pds">'</span></span>);
  },
  <span class="pl-en">render</span><span class="pl-k">:</span> <span class="pl-k">function</span>() {
    <span class="pl-k">return</span> (<span class="pl-k">&lt;</span>div className<span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>page<span class="pl-pds">"</span></span> onClick<span class="pl-k">=</span>{<span class="pl-v">this</span>.clickEvent}<span class="pl-k">&gt;</span>
      Hello world
    <span class="pl-k">&lt;</span>/div<span class="pl-k">&gt;</span>);
  }
});</pre>

</code></pre></td></tr></tbody></table>

<!--
```jsx
<template name="Page">
  <div class="page">
    Hello world
  </div>
</template>

Template.Page.events({
  'click .page': function() {
    console.log('Hello world');
  }
});
```

```jsx
Page = React.createClass({
  clickEvent: function() {
    console.log('Hello world');
  },
  render: function() {
    return (<div className="page" onClick={this.clickEvent}>
      Hello world
    </div>);
  }
});
```
-->

### Iterating through a list with click handlers

#### Sideburns (.html.jsx)

```jsx
<template name="Page">
  <ul>
    {{#each person in people}}
    <li class="item {{isSelected}}">{{person.name}}</li>
    {{/each}}
  </ul>
</template>

Template.Page.onCreated(function() {
  this.selected = new ReactiveVar(false);
});

Template.Page.helpers({
  people() {
    return People.find();
  },

  isSelected: function(context) {
    return this.selected.get() == context._id ? 'active' : '';
  }
});

Template.Page.events({
  'click ul li': function(context, event) {
    this.selected.set(context._id);
  }
});
```

#### React comparison

```jsx
Page = React.createClass({
  mixins: [ReactMeteorData],
  getMeteorData() {
    return {
      people() {
        return People.find();
      }
    }
  },
  getInitialState() {
    return {selected: false};
  },
  isSelected(context) {
    return this.state.selected == context._id ? 'active' : '';
  },
  clickHandler(context, event) {
    this.setState({selected: context._id});
  },
  render() {
    return (<ul>{
      this.data.people.map((person, index) => {
        return (<li key={index} onClick={this.clickHandler.bind(this, person)}
          className={'item ' + this.isSelected(person)}>{person.name}</li>);
      });
    }</ul>);
  }
});
```

<!-- Table template for comparisons.
<table width="100%"><thead><tr><th width="50%">Sideburns (.html.jsx)</th><th width="50%">React comparison</th></tr></thead><tbody><tr><td valign="top"><pre lang="jsx"><code>

</code></span></pre></td><td valign="top"><pre lang="jsx" class="vicinity rich-diff-level-zero"><code>

</code></pre></td></tr></tbody></table>
-->

## Features

- [x] .html.jsx templates
- [x] Blaze helpers
- [ ] Blaze helper context
- [x] Blaze onCreated
- [x] Blaze events
- [ ] Blaze onRendered
- [ ] Blaze onDestroyed
- [ ] Blaze autorun
- [ ] Blaze subscribe
- [x] Spacebars {{helper}} (SafeString)
- [x] Spacebars {{{helper}}} (raw HTML)
- [x] Spacebars "{{helper}}" (SafeString – In Attribute Values)
- [x] Spacebars ={{helper}} (SafeString – Dynamic Attribute Value)
- [x] Spacebars {{#if}}
- [x] Spacebars {{else}} {{/if}}
- [ ] Spacebars {{#each}}
- [x] Spacebars {{#each in}}
- [ ] Spacebars {{else}} {{/each}}
- [ ] Spacebars {{#with}}
- [ ] Spacebars {{else}} {{#with}}
- [x] Spacebars {{#unless}}
- [x] Spacebars {{else}} {{/unless}}

## PHASE 2 CONSIDERATIONS

 - Events in blaze are applied to not just the current template, but child templates as well. We need to pass down **ALL** event handlers to child components dynamically at runtime. 
 - `Template.instance().foo = new ReactiveVar('bar')` should be transpiled to `this.setState({}, {foo: 'bar'}, this.getState())`. ReactiveDict should become sub objects on our state object. And of course the getters should matchup as well. 
 - dynamic template rendering in JS needs to be addressed, as well as `Template.dynamic` in spacebars
 - `currentData` should be addressed, but `parentData` should likely not be allowed
 - `$/find/firstNode/etc` needs to be addressed. It seems `refs` are the way to go. Does that mean we give every element a ref. I'm thinking we only support a subset of what jQuery can do here so u can find the first and last node, and maybe `$('#id')`. Most of the time you only use this stuff to get the value out of an input element. We basically shouldn't expect to shim everything, but handle common use cases, and propose a "transition path" which outlines the most minimal set of alterations developers must take to make their code work with sideburns. The main recommendation being to use `refs`. We can perhaps provide a shared interface in both Blaze and a wrapped version of React we produce. So you can do `this.refs.foo` and in Blaze `refs` a getter is using jquery under the hood: `return $('[ref]')...` where ultimately that code will return an object whose keys are ref names and the values, the nodes. 
 - In Blaze 1.0 the instance context for event handlers is the template where it was fired, even though it may be defined on a parent template. As mentioned in the first Phase 2 Consideration, we need to pass these handlers down--however the instance context will be that of the react component. However in my Blaze 2.0, the instance context is the initial "blaze component" where it was defined. This solves a major problem along with a matching solution for helpers. The problem is that because of this you cant do `Template.instance()` in an event handler and reliably receive the same instance as you would get when called in a helper! You also can't simply apply a click handler to a child template and refer to instance data of a parent instance. You end up using `parentData` and the unofficial `parentInstance` several developers have done (which is not unidirectional) and has led to major problems for people. So My Blaze 2.0 binds the instance context reliably to events even when triggered in child templates, and *in addition* passes down helpers to child templates--that way they can both reliably refer to the same instance context, aka "state." What this makes for is the "controller component" pattern in Blaze. So when you want to share state between callbacks, helpers and event handlers across a tree of of parent + child components, you move these helper and event methods to the highest parent component that requires the state. This is a lot more like React to begin with. You also get methods such as `this.setState` and `this.props` (a getter under the hood) which mimics react. What I have in mind is that we abstract out the Blaze 1.0 aspects of Sideburns and maintain an underlying package that both Blaze 1.0 and my Blaze 2.0 can use. That way they can share code easily. What I'm going to need to do is an extension of what I mention above about passing event handlers down. Basically I gotta pass down both event handlers and helpers, but as `props` essentially whose context was bound to the component that had them defined. This is unlike your event handlers which must be passed down, but who use the component instance context of the component where they occur! Accomplishing both may in fact be very similar. In both cases we simply have to pass down every single handler/helper dynamically at runtime, and combine them with other explicitly passed props. Obviously the explicitly passed props and helpers/handlers of child components with the same name override ones passed from parents--thats how Blaze 2.0 works as well. Other than that, all I really gotta do is make it so I extract helpers, events and callbacks off my classes rather than Blaze maps, which is I'm proposing we move some of htis code into an underlying package so both Blaze 1.0 and Blaze 2.0 can share it. In short, each get their methods from different places, and bind different contexts, but both need the same mechanism to pass all these methods down dynamically at runtime (minus helpers for Blaze 1.0). Lastly, Blaze 2.0 in fact has one major thing that's a lot easier: code like `Template.instance()` doesn't need to be compiled since it already has a React interface for such things! So the corresponding code would go in the Blaze 1.0 package, but not the underlying shared library. 
