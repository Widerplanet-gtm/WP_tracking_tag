# Widerplanet Tracking tag

[Document_link](https://github.com/Widerplanet-gtm/WP_tracking_tag/blob/master/gtm_guide.pdf)

``` javascript
const copyFromWindow = require('copyFromWindow');
const injectScript = require('injectScript');
const log = require('logToConsole');

scriptCall(data.EventType);

function scriptCall(EventType) {
  injectScript('https://cdn-aitg.widerplanet.com/js/wp_astg_6.0.js', function() {
    const wp_items = copyFromWindow('wp_items');
    const wpts = copyFromWindow('wpts');
    
    wpts.init(data.ClientID);
    
    //debug log
    //log('ClientID =', data.ClientID);
    //log('EventType =', EventType);
    //log('wp_items =', wp_items);
    
    if (EventType == 'Home') {
      wpts.tag();
    } else if (EventType == 'Login' || EventType == 'Join') {
      wpts.tag({
        ty: EventType,
        items: [ {i:EventType, t:EventType, p:1, q:1} ]
      });
    } else if (EventType == 'Item' || EventType == 'Cart' || EventType == 'PurchaseComplete') {
      wpts.tag({
        ty: EventType,
        items: wp_items
      });
    }
    
  }, function(err) {
    log('inject error:' + err);
  });
}

// 태그가 종료되는 시점에 data.gtmOnSuccess를 호출하세요.
data.gtmOnSuccess();
```
