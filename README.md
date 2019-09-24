# react-html-replace

A simple way to safely do html tag replacement with React components

## Install

```
$ npm install --save react-html-replace
```

## Usage

### Simple Example

```js
import React, { Component } from 'react';
import { render } from 'react-dom';

import reactHtmlReplace from 'react-html-replace';

const Mention = ({ children, id, name }) => {
  return (
    <span name={name} id={id}>
      {children}
    </span>
  );
};

const HashTag = ({ tag }) => <span>{`#${tag}`}</span>;

class Demo extends Component {
  render() {
    return (
      <div>
        {reactHtmlReplace(
          `<italic>This is <bold> xml string</bold> with custom nexted markup,<bold> we can get inner markup & attribute  through props.</bold></italic> <mention id ="123" name ="raodurgesh">  this is mention tag with id & name attribute </mention> <hashtag tag="howdymody"></hashtag>`,
          (tag, props) => {
            if (tag === 'bold') {
              return <b />;
            }
            if (tag === 'italic') {
              return <i />;
            }
            if (tag === 'mention') {
              const { name, id, innertext } = props;
              return (
                <Mention name={name} id={id}>
                  {innertext}
                </Mention>
              );
            }
            if (tag === 'hashtag') {
              const { tag } = props;
              return <HashTag tag={tag} />;
            }
          }
        )}
      </div>
    );
  }
}

render(<Demo />, document.querySelector('#demo'));
```

### Output

<i>This is <b> xml string</b> with custom nexted markup,<b> we can get inner markup &amp; attribute through props.</b></i> <span name="raodurgesh" id="123" style="border: 1px solid rgb(204, 204, 204); padding: 0px 10px;"> this is mention tag with id &amp; name attribute </span>

## Params

`import reactHtmlReplace from 'react-html-replace';`

`reactHtmlReplace(`**xmlstring**, **callbackfunction**`);`

**xmlstring** : _the html string must have opening and closing tags._

**callbackfunction** :

- tag : _(html custom tag)_
- Props : _Attributes of tag and innerhtml between the tag_

i.e ,

**xmlstring** :`<bold> This is </bold><link href = "https://github.com">demo</link>`

**callbackfunction** : `(tag, props)`:

**tag** :`bold , link`

**props** : `innertext, href`
