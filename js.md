```javascript
// ==UserScript==
// @name              鱼C论坛
// @namespace         https://soulsign.inu1255.cn?account=ViCrack
// @version           1.0.1
// @author            821218213
// @loginURL          https://fishc.com.cn
// @updateURL         https://soulsign.inu1255.cn/script/ViCrack/鱼C论坛
// @expire            900000
// @domain            fishc.com.cn
// ==/UserScript==

exports.run = async function(param) {
// 签到的页面
    let resp = await axios.get("https://fishc.com.cn/plugin.php?id=k_misign:sign");
    let signhtml = resp.data;
    if (signhtml.includes("您的签到排名")){
        return "已经签到过";
    }
    let result = signhtml.match(/<a id="JD_sign" href="(.*?)"/);
    if (result == null) {
        throw "未登录";
    }
    let signurl = result[1];
    var {
        data
    } = await axios.get(
        "https://fishc.com.cn/" + signurl
    );
    if (/今日已签/.test(data)) return "任务已完成";
    if (/需要先登录/.test(data)) throw "未登录";
    
    return "签过了";
};

exports.check = async function(param) {
var {
        status,
        data,
    } = await axios.get(
        "https://fishc.com.cn/home.php?mod=spacecp&ac=usergroup", {
            maxRedirects: 0
        }
    );
    if (/需要先登录/.test(data)) return false;
    return true;
};
```
