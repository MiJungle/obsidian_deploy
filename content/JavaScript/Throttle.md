```js
const Progress = function () {
    this.buttonSyncData = $(".buttonSyncData");

    this.onClickSyncData = function (){
        const throttleSyncData = throttle(() => _global.device.requestSyncData(), 1000);
        this.buttonSyncData.off("click").on("click", function() {
            let isAppYn = _global.common.getStorageIsAppYn();
            isAppYn === "Y"? _global.device.requestSyncData() : _global.progress.showWebSyncModal();
            isAppYn === "Y" ? throttleSyncData() :  _global.progress.showWebSyncModal();
        })
    }

```


```js
function throttle(func, limit) {
    let inThrottle = false;
    return function() {
        if (!inThrottle) {
            func();
            inThrottle = true;
            setTimeout(() => {
                inThrottle = false;
            }, limit);
        }
    };
}
```