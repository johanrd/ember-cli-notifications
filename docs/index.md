<div class="bg-blue white py4">
  <div class="max-width-4 mx-auto px2">
    <div class="mb3">
      <h1 class="m0">ember-cli-notifications</h1>
      <p class="h3 muted">Atom inspired notifications for ember-cli</p>
    </div>
    <div class="flex mxn1">
      <div class="px1">
        <a class="btn btn-outline" href="https://github.com/mansona/ember-cli-notifications" target="_blank" rel="noopener">GitHub</a>
      </div>
      <div class="px1">
        <a class="btn btn-outline" href="https://www.npmjs.com/package/ember-cli-notifications" target="_blank" rel="noopener">npm</a>
      </div>
    </div>
  </div>
</div>
<div class="max-width-4 mx-auto px2">
  <div class="py2">
    <h2>Installation</h2>
    <hr>

```bash
ember install ember-cli-notifications
```

  </div>
  <div class="py2">
    <h2>Basic usage</h2>
    <hr>
    <p>Include the container component in your template.</p>
    <p>Optionally, change the position or z-index value of the notifications container.</p>
    <p class="h5"><i>Default value is top</i></p>

```handlebars{data-execute=false}
{{notification-container position="top-right" zindex="9999"}}
```

    <p>Inject the notifications service where required.</p>

```js
import Controller from '@ember/controller';
import { inject as service } from '@ember/service';

export default Controller.extend({
  notifications: service(),
});
```

    <p>Or inject it everywhere with an initializer.</p>

```js
// app/initializers/inject-notifications.js
export function initialize(application) {
  ['controller', 'component', 'route'].forEach(injectionTarget => {
    application.inject(injectionTarget, 'notifications', 'notification-messages:service');
  });
};

export default {
  name: 'inject-notifications',
  initialize
};
```

    <p>There are four styles of notification available.</p>
    <p class="h5"><i>Default value is info</i></p>

```js
this.notifications.info('You have one unread message');

// Error
this.notifications.error('Something went wrong');

// Success
this.notifications.success('Saved successfully!');

// Warning
this.notifications.warning('You have unsaved changes');
```

    <h3>htmlContent</h3>
    <p>You can enable basic HTML markup for notification messages.</p>
    <p><b>Warning:</b> This introduces a potential security risk since the notification message will no longer be escaped by Ember.</p>

```js
this.notifications.info('You have <b>one</b> unread message', {
  htmlContent: true
});
```

    <div class="bg-darken-1 rounded p2 mb3">
      <div class="mxn2 mb3">
        <div class="px2">
          <h4>Message</h4>
          {{#if this.htmlContent}}
            <Input class="input" @type="text" @value={{this.htmlMessage}} />
          {{else}}
            <Input class="input" @type="text" @value={{this.message}} />
          {{/if}}
          <div class="mt1">
            <label class="h5">
              <Input @type="checkbox" @checked={{this.htmlContent}} />
              Enable HTML content
            </label>
          </div>
        </div>
        <div class="px2">
          <h4>Type</h4>
          <div data-test-radio-html class="sm-flex mxn1">
            <div class="px1 mb1">
              <label>
                <RadioButton @value="success" @checked={{this.type}} />
                Success
              </label>
            </div>
            <div class="px1 mb1">
              <label>
                <RadioButton @value="info" @checked={{this.type}} />
                Info
              </label>
            </div>
            <div class="px1 mb1">
              <label>
                <RadioButton @value="error" @checked={{this.type}} />
                Error
              </label>
            </div>
            <div class="px1 mb1">
              <label>
                <RadioButton @value="warning" @checked={{this.type}} />
                Warning
              </label>
            </div>
          </div>
        </div>
        <div class="px2">
          <h4>Position</h4>
          <div class="sm-flex mxn1">
            <div class="px1 mb1">
              <label>
                <RadioButton @value="top" @checked={{this.position}} />
                top
              </label>
            </div>
            <div class="px1 mb1">
              <label>
                <RadioButton @value="top-left" @checked={{this.position}} />
                top-left
              </label>
            </div>
            <div class="px1 mb1">
              <label>
                <RadioButton @value="top-right" @checked={{this.position}} />
                top-right
              </label>
            </div>
            <div class="px1 mb1">
              <label>
                <RadioButton @value="bottom" @checked={{this.position}} />
                bottom
              </label>
            </div>
            <div class="px1 mb1">
              <label>
                <RadioButton @value="bottom-left" @checked={{this.position}} />
                bottom-left
              </label>
            </div>
            <div class="px1 mb1">
              <label>
                <RadioButton @value="bottom-right" @checked={{this.position}} />
                bottom-right
              </label>
            </div>
          </div>
        </div>
      </div>
      <button class="btn btn-primary" data-test-button-html-show {{on "click" this.showNotifcation}}>Show</button>
    </div>
  </div>
  <div class="py2">
    <h2>Clearing notifications</h2>
    <hr>
    <h3>autoClear</h3>
    <p>Set a notification to automatically clear. This will take precedence over the global default value.</p>
    <p>If true, inherits the default <b>clearDuration</b> value unless specified.</p>
    <p class="h5"><i>Default value is false</i></p>

```js
this.notifications.success('Saved successfully!', {
  autoClear: true
});
```

    <h3>clearDuration</h3>
    <p>Specify a duration (ms) before the notification clears. This will take precedence over the global default value.</p>
    <p class="h5"><i>Default value is 3200</i></p>

```js
this.notifications.success('Saved successfully!', {
  autoClear: true,
  clearDuration: 1200
});
```

    <h3>clearAll</h3>
    <p>You can remove all present notifications before adding a new one.</p>

```js
this.notifications.clearAll().success('Saved successfully!', {
  autoClear: true
});
```

    <div class="bg-darken-1 rounded p2 mb3">
      <div class="mxn2 mb3">
        <div class="px2">
          <h4>Clear automatically</h4>
          <label>
            <Input @type="checkbox" @checked={{this.autoClear}} />
            Enable
          </label>
          <div class="mt1">
            <label class="label">Timeout (ms)</label>
            <Input class="input" @type="number" @value={{this.clearDuration}} disabled={{this.disableTimeoutInput}} />
          </div>
        </div>
        <div class="px2">
          <h4>Clear all</h4>
          <label>
            <Input @type="checkbox" @checked={{this.clearAll}} />
            Enable
          </label>
        </div>
      </div>
      <button class="btn btn-primary" {{action "showNotifcation"}}>Show</button>
    </div>
  </div>
  <div class="py2">
    <h2>Notification with custom CSS</h2>
    <hr>
    <p>Pass a string of CSS classes to the notification component.</p>

```js
this.notifications.success('Saved successfully!', {
  autoClear: true,
  cssClasses: 'profile-saved-success-notification'
});
```

  </div>
  <div class="py2">
    <h2>Notification with callback</h2>
    <hr>
    <p>Add the ability to click the notification for a callback.</p>

```js
this.notifications.error('Something went wrong. Click to try again', {
  onClick: (notification) => {
    this.get('model').save();
  }
});
```
  </div>
    <div class="py2">
    <h2>Notification with callback on dismiss button</h2>
    <hr>
    <p>Add the ability to add a callback when dismiss button is clicked.</p>

```js
this.notifications.error('Something went wrong. Click to try again', {
  onDismiss: (notification) => {
    this.hasClickedDismissOnError = true
  }
});
```
  </div>
  <div class="py2">
    <h2>Config</h2>
    <hr>
    <h3>Globals</h3>
    <p>Set the global default values for <b>autoClear</b> and <b>clearDuration</b>.</p>

```js
// config/environment.js
var ENV = {
  'ember-cli-notifications': {
    autoClear: true,
    clearDuration: 2400
  }
}
```
  </div>

  <div class="py2">
    <h2>Extend</h2>
    <hr>
    <p>The <b>notifications</b> service can be imported into the consuming app and extended.</p>
```js
// app/services/notifications.js
import NotificationsService from 'ember-cli-notifications/services/notifications';

export default NotificationsService.extend({
  // Extend the imported service
});
```

    <p>Be sure to inject the <i>new</i> service into your app.</p>
  </div>
</div>

<NotificationContainer @position={{this.position}} @zindex={{this.zindex}} />
