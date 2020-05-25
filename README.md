## Create a new React component

Forked from https://github.com/joshwcomeau/new-component.
Added Storybook and basic testing.

### Usage

Install via NPM:

#### Globally for all projects

```bash
# Using Yarn:
$ yarn global add @metamn/new-component

# or, using NPM
$ npm i -g @metamn/new-component
```

#### Locally for a specific project

```bash
# Using Yarn:
$ yarn add @metamn/new-component

# or, using NPM
$ npm i @metamn/new-component
```

Then run:

```bash
$ cd PROJECT_DIRECTORY
$ new-component Button
```

### Special edition

This version of `new-component` is a special edition supporting translations with `i18next` at component level.

#### Setup

You must have a `i18n.js` config file next to the `index.js` file in a CRA app.

```js
import i18n from "i18next";
import { initReactI18next } from "react-i18next";
import LanguageDetector from "i18next-browser-languagedetector";

/**
 * Sets up translations
 *
 * - Every component manages it's own translations via namespaces
 * - Every component adds it's own translation items to `resources`
 *
 * @see https://github.com/i18next/react-i18next/issues/299
 * @see https://react.i18next.com/latest/usetranslation-hook#loading-namespaces
 *
 */
const resources = {};

/**
 * Sets up i18next
 *
 * @see https://react.i18next.com/latest/using-with-hooks
 */
i18n
  /**
   * Detects user language
   *
   * @see https://github.com/i18next/i18next-browser-languageDetector
   */
  .use(LanguageDetector)
  /**
   * Passes the i18n instance to react-i18next.
   */
  .use(initReactI18next)
  /**
   * Inits i18next
   *
   * @see https://www.i18next.com/overview/configuration-options
   */
  .init({
    resources,
    lng: "en-EN",

    keySeparator: false, // we do not use keys in form messages.welcome

    interpolation: {
      escapeValue: false // react already safes from xss
    }
  });

export default i18n;
```

In `index.js` you'll have to add this import statement:

```js
import "./i18n"; // See https://react.i18next.com/latest/using-with-hooks
```

### What you'll get

In `src/components/Button`:

```Javascript
// `Button/Button.js`
import React from "react";
import PropTypes from "prop-types";
import styled from "styled-components";

import i18n from "../../i18n";
import { useTranslation } from "react-i18next";
import { en_en } from "./Button.lang.en-en";
import { nl_nl } from "./Button.lang.nl-nl";
import { nl_be } from "./Button.lang.nl-be";

i18n.addResourceBundle("en-EN", "Button", en_en);
i18n.addResourceBundle("nl-NL", "Button", nl_nl);
i18n.addResourceBundle("nl-BE", "Button", nl_be);

/**
 * Defines the prop types
 */
const propTypes = {};

/**
 * Defines the default props
 */
const defaultProps = {};

/**
 * Styles the component container
 */
const Container = styled("div")(props => ({
  border: "1px solid",
  padding: "1.25em",
  margin: "1.25em"
}));

/**
 * Displays the component
 */
const Button = props => {
  const { t } = useTranslation("Button");
  return (
    <Container className="COMPONENT_NAME">{t("COMPONENT_NAME")}</Container>
  );
};

Button.propTypes = propTypes;
Button.defaultProps = defaultProps;

export default Button;
export { propTypes as ButtonPropTypes, defaultProps as ButtonDefaultProps };
```

```Javascript
// `Button/Button.stories.js`
import React from "react";
import Button from "./Button";
import ApiDoc from "./Button.md";

export default {
  component: Button,
  title: "Button",
  parameters: { notes: ApiDoc }
};

export const Default = () => <Button />;
```

```Markdown
// `Button/Button.md`
# Button
```

```Javascript
// `Button/Button.test.js`
import React from "react";
import { render } from "@testing-library/react";
import Button from "./Button";

it("has a Button component", () => {
  const { getByText } = render(<Button />);
  expect(getByText("Button")).toBeInTheDocument();
});
```

```Javascript
// `Button/index.js`
export { default, ButtonPropTypes, ButtonDefaultProps } from "./Button";
```

```Javascript
// `Button/Button.lang.en-en.js`
const en_en = {
  Button: "Button"
};

export { en_en };
```

```Javascript
// `Button/Button.lang.nl-nl.js`
const nl_nl = {
  Button: "Button (nl_nl)"
};

export { nl_nl };
```

```Javascript
// `Button/Button.lang.nl-be.js`
const nl_be = {
  Button: "Button (nl_be)"
};

export { nl_be };
```

### Modify & test locally

You can easily fork this repo, modify, test, and publish on npm.

NOTE: Always start with changing the version number in `package.json` !!

#### Test locally

In this current repo:

```shell
npm pack
```

This will create a file like `new-component@0.0.1.tgz`.

In another folder:

```bash
npm i <path_to_new_component_repo>/new-component@0.0.1.tgz &&
./node_modules/.bin/new-component Button
```

#### Update the changelog

NOTE: Always update the changelog

#### Publish

First push changes to Github. Then:

```bash
npm publish
```

## Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

### [0.3.0] - 2020-01-14

#### Added

- `styled-components`, re-added, because when a new component is added it's first purpose is to mockup. Also every new component has a border and padding added to serve mockuping.
- `react-i18next` with `addResourceBundle` loading translations from language files co-located to the component

### [0.2.0] - 2019-11-6

#### Added

- Default tests

#### Changed

- Storybook stories to follow the Component Story Format: https://storybook.js.org/docs/formats/component-story-format/

#### Removed

- `styled-components`, because many times components use another library, like `material-ui`

### [0.1.2] - 2018-08-17

#### Fixed

- Exporting default props

### [0.1.1] - 2018-08-17

#### Changed

- How default props are exported

### [0.1.0] - 2018-07-03

#### Added

- New templates for function components

### [0.0.4] - 2018-12-11

#### Fixed

- Markdown is not run through `prettify()`.

### [0.0.3] - 2018-12-11

#### Added

- Markdown documentation support.

### [0.0.2] - 2018-12-11

#### Added

- A CHANGELOG section in README.

#### Changed

- The install instructions in README.

### 0.0.1 - 2018-12-11

#### Added

- Initial release
