# Protactor Test Framework


## Config.js
Here is an example config file:
```js
exports.config = {
    chromeDriver: './support/chromedriver', // relative path to node-webkit's chromedriver
    chromeOnly: true, // starting Selenium server isn't required in our case
    seleniumAddress: 'http://localhost:4444/wd/hub',
    specs: ['todo-spec.js'],
    rootElement: 'html', // specify a correct element where you bootstrap your AngularJS app, 'body' by default

    onPrepare: function() {

        // By default, Protractor use data:text/html,<html></html> as resetUrl, but
        // location.replace from the data: to the file: protocol is not allowed
        // (we'll get ‘not allowed local resource’ error), so we replace resetUrl with one
        // with the file: protocol (this particular one will open system's root folder)
        browser.resetUrl = 'file://';

        // This isn't required and used to avoid ‘Cannot extract package’ error showed
        // before Protractor have redirected node-webkit to resetUrl.
        browser.driver.get('file://');
    }
};

```


You can use file paths instead of a server page.

```js
exports.config = {
    // ...

    baseUrl: 'file:///absolute/path/to/your/project/index.html',

    onPrepare: function() {

        // By default, Protractor use data:text/html,<html></html> as resetUrl, but
        // location.replace from the data: to the file: protocol is not allowed
        // (we'll get ‘not allowed local resource’ error), so we replace resetUrl with one
        // with the file: protocol (this particular one will open system's root folder)
        browser.resetUrl = 'file://';
    }

    // ...
};
```

If you want to run a relative path to your project folder, then you could just use Node.js tools, because Protractor runs in a Node.js environment. For example, `__dirname` will return an absolute path to a directory where your Protractor config file is saved. As a result use:

```js
exports.config = {
    // ...

    baseUrl: 'file://' + __dirname + '/spec/support/index.html'

    // ...
};
```

Also, if you application does XHR requests to some endpoints, which are not allowed from file:, you may have to run your test browser with custom flags. In my case it was Chrome:

```js
exports.config = {
    // ...

    capabilities: {
        browserName: 'chrome',
        chromeOptions: {
            // --allow-file-access-from-files - allow XHR from file://
            args: ['allow-file-access-from-files']
        }
    }

    // ...
}
```

## Angular-less

Use `browser.driver` instead of `driver` and also set `browser.driver.ignoreSynchronization = true` so it does not wait on angular to load.

### Find elements
* Find elements by Id: `browser.driver.findElement(by.id('elem_id'));`
* Find elements by name: `browser.driver.findElement(by.name('elem_name'));`
* Find elements by xpath: `browser.driver.findElement(by.xpath('elem_xpath'));`
* Find elements by css Selector: `browser.driver.findElement(by.css('cssSelector));`
* Find elements by tag name: `browser.driver.findElement(by.tagName('tagName'));`
* Find elements by class name `browser.driver.findElement(by.className('class_name'));`
* Find elements by link text: `browser.driver.findElement(by.linkText('link_text'));`
* Find elements by partial link text: `browser.driver.findElement(by.partialLinkText ('partial_link_text'));`
