<template>
  <div id="app">
    <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
    <ResumeEditor
      ref="resumeEditor"
      :markdown="currentMarkdown"
      :enableHtml="enableHtml"
    ></ResumeEditor>
  </div>
</template>
<script>
import StyleEditor from "./components/StyleEditor";
import ResumeEditor from "./components/ResumeEditor";
import "./assets/reset.css";
let isPc = (function () {
  var userAgentInfo = navigator.userAgent;
  var Agents = [
    "Android",
    "iPhone",
    "SymbianOS",
    "Windows Phone",
    "iPad",
    "iPod",
  ];
  var flag = true;
  for (var v = 0; v < Agents.length; v++) {
    if (userAgentInfo.indexOf(Agents[v]) > 0) {
      flag = false;
      break;
    }
  }
  return flag;
})();
let getDateDiff = function (startDate, endDate) {
  var startTime = new Date(Date.parse(startDate.replace(/-/g, "/"))).getTime();
  var endTime = new Date(Date.parse(endDate.replace(/-/g, "/"))).getTime();
  var dates = Math.abs(startTime - endTime) / (1000 * 60 * 60 * 24);
  return dates;
};
document.title +=
  getDateDiff(
    new Date().getFullYear() +
      "-" +
      (new Date().getMonth() + 1) +
      "-" +
      new Date().getDate(),
    "2020-12-22"
  ) + "天";
export default {
  name: "app",
  components: {
    StyleEditor,
    ResumeEditor,
  },
  data() {
    return {
      interval: 27,
      currentStyle: "",
      enableHtml: false,
      fullStyle: [
        `/*
* Hi。我的宝！爱你哦❤️️
* 很抱歉让你一次次失望。心痛。
* 是我自己把你推走的
* 我希望我可以用最后的方式
* 对你表达我对你的爱
* 我终于改变的思想
* 你是如此的好的一个女孩子
* 可是我一直从心里都很爱你
* 并不是因为你爱我，我才爱你
* 而是我本来就真的很爱你
* 你能不能不要跟我分开
* 说起来手机和电脑还得区分一下。
* 我猜你现在用的是。。。${isPc ? "电脑" : "手机"}
* 宝，我猜对了嘛？是不是很聪明
*/

/* 首先给所有元素加上过渡效果 */
* {
  -webkit-transition: all .3s;
  transition: all .3s;
}
/* 白色背景太单调了。来点潇潇老婆喜欢的绿色的背景，毕竟老婆说除了粉色喜欢的就是绿色了 */
html {
  color: rgb(240,240,240);
  background: rgb(46 ,139, 87); 
}
/* 文字太近了，影响我老婆观看，调整一下 */
.styleEditor {
  padding: .5em;
  border: 1px solid;
  margin: .5em;
  overflow: auto;
  ${isPc ? "width: 48%;height: 96%;" : "width: 96%;height: 50%;"}
  font-size: 14px;
  line-height:1.5;
}
/* 这些代码颜色都一样。加点儿高亮区别来 */
.token.selector{ color: rgb(133,153,0) }
.token.property{ color: rgb(187,137,0) }
.token.punctuation{ color: yellow }
.token.function{ color: rgb(42,161,152) }
.token.comment{ color: rgb(177,177,177) }
/* 加个 3D 效果 */
html{
  -webkit-perspective: 1000px;
          perspective: 1000px;
}
.styleEditor {
  position: fixed; 
  ${isPc ? "left: 0;" : "left:0;right:0;margin:auto;"}
  top: 0; 
  -webkit-transition: none; 
  transition: none;   
  ${
    isPc
      ? "-webkit-transform: rotateY(10deg) translateZ(-100px) ;transform: rotateY(10deg) translateZ(-100px) ;"
      : "-webkit-transform: rotateX(-10deg) translateZ(-100px) ;transform: rotateX(-10deg) translateZ(-100px) ;"
  }
  ${
    isPc ? "" : "-webkit-transform-origin: 50% 0% 0;transform-origin: 50% 0% 0;"
  }
}

/* 再来一张情书💌️背景 */
.resumeEditor{
  position: fixed; 
  ${isPc ? "right: 0;" : "left:0;right:0;margin:auto;"}
  ${isPc ? "top: 0;" : "bottom:2%;"}
  padding: .5em;  
  ${isPc ? "margin: .5em;" : ""}
  ${isPc ? "width: 48%;height: 96%;" : "width: 96%;height: 50%;"}
  border: 1px solid;
  color: #222;
  overflow: auto;
  font-size: 14px;
  line-height:1.5;
  ${
    isPc
      ? "-webkit-transform: rotateY(-10deg) translateZ(-100px) ;transform: rotateY(-10deg) translateZ(-100px) ;"
      : "-webkit-transform: rotateX(10deg) translateZ(-100px) ;transform: rotateX(10deg) translateZ(-100px) ;"
  }
    ${
      isPc
        ? ""
        : "-webkit-transform-origin: 50% 0% 0;transform-origin: 50% 0% 0;"
    }
  }
/* 老婆。我要开始写了哦~ */


`,
        `
/* 是不是看着很简陋粗糙？
 * 因为这是 Markdown 格式的
 * 一种程序员用来写文档日志的简易语言
 * 翻译成 网页 就行了
 */
`,
        `
/* 再潇潇老婆加点样式 */
.resumeEditor{
  padding: 2em;
  line-height:1.8;
}
.resumeEditor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
  font-size:18px;
}
.resumeEditor ul,.resumeEditor ol{
  list-style: none;
}
.resumeEditor ul> li::before{
  content: '•';
  margin-right: .5em;
}
.resumeEditor ol {
  counter-reset: section;
}
.resumeEditor ol li::before {
  counter-increment: section;            
  content: counters(section, ".") " ";  
  margin-right: .5em;
}
.resumeEditor blockquote {
  margin: 1em;
  padding: .5em;
  background: rgba(221,221,221,.5);
}

/* OK。完成！ Mua！ 爱老婆❤️️ */

`,
      ],
      currentMarkdown: "",
      fullMarkdown: `牟明写给王潇琦的第一份电子情书
----

2020年10月。初次见面。大五憨憨学长和大一可爱学妹的认识。  
2020年12月22日。即庚子鼠年冬月初九。在一起，爱老婆。  
我们已经在一起 \`${getDateDiff(
        new Date().getFullYear() +
          "-" +
          (new Date().getMonth() + 1) +
          "-" +
          new Date().getDate(),
        "2020-12-22"
      )}\` 天
2021年5月10日我和宝真的要分开了，今天我好难过，可是我还是希望我宝能再看看我，不要赶我走，我真的好爱我宝。
迄今为止一起做过的事
----
* 一起ghs
* 一起吃饭
* 一起逛商场
* 一起打电话睡觉
* 一起抱着睡觉
* 一起看电影
* 一起接吻
* 一起喝一杯水
* 一起坐地铁
* 一起去游乐园
* 一起去街机厅
* 一起逛东软校园
* 一起上课
* 一起吃食堂
* 我去了她在的城市
* 她来了我在的城市
* 一起逛了书店
* 看了不多但是有几场的电影
* 一起过了一次情人节，即使过的很坏
* 一起用了很多不同功能的避孕套
* 将要尝试情趣小玩具和情趣内衣

和凑凑老婆一起玩过的游戏
----
* 和平精英
* 人类一败涂地

想对凑凑老婆说的话
----
    亲爱的凑凑老婆，潇琦宝贝，我和你一起度过了第一个新年我很开心，接着今天是2月14日虽然是凌晨但是今年的情人节我也是跟潇潇老婆过，
    真的超级开心和潇潇老婆在一起的这两个月里面，我们经历了无数的争吵，分手，冷静，伤害，失望。但是老婆都包容了我，
    我也积极检讨自己索性没有在这个情人节前分手，呜呜呜，不想和我的凑凑老婆分手，凑凑老婆我爱你，笔芯，mua~，
    「**入目与他人，四下皆是你**」。
    我知道异地恋很辛苦，看到晚上你发的那些真的很难过，就不知道怎么回复你，总之是这个东西也不想再做了，很生气也很无奈，
    我不知道说什么，就觉得我做的这个东西很无力，可是我也不能怎么办，打开手机看了看去武山的火车票是晚上9点，汽车票也没了，
    又关了手机，心里很不是滋味，唉，或许异地恋就是如此的难过吧。
    我也很想潇潇老婆，我也希望能多见潇潇老婆几面，要是能和潇潇老婆一直在一起就好了，这是第一次给老婆准备情人节礼物，如果做的不好还请见谅。
    爱你哦~ 凑凑老婆。
****************************************************************************
    今天是想有一肚子的话想对老婆说，不知道以后还能不能见到我老婆，宝，我真的一点都不想把你让给别人，我就是累死，折磨死我也不想把你让给别人，
    我没办法想象你为了别人穿情趣内衣，我没办法想象你跟别人躺在一张床上，我真的没办法想象，你说的每句话都让我心痛，我真的一点也不想离开你，
    我好爱你啊，我自己都没发现，我远比自己想象中的爱你，我极力的想要挽回你，可是你真的好绝情，我真的好难过，难过到喘不过气，难过到自己快要窒息了！
    宝，臭宝，我真的不能没有你
    宝，宝，宝

最后和老婆的愿景
----

> 【Screw the world × I have my dear xiaoxiao】  
>   即使天无雨，我亦在此地，但盼风雨来，能留你在此。
>> 臭宝，不要离开我好不好，我已经改了，从思想上改了，臭宝
-----
`,
    };
  },
  created() {
    this.makeResume();
  },

  methods: {
    makeResume: async function () {
      await this.progressivelyShowStyle(0);
      await this.progressivelyShowResume();
      await this.progressivelyShowStyle(1);
      await this.showHtml();
      await this.progressivelyShowStyle(2);
    },
    showHtml: function () {
      return new Promise((resolve, reject) => {
        this.enableHtml = true;
        resolve();
      });
    },
    progressivelyShowStyle(n) {
      return new Promise((resolve, reject) => {
        let interval = this.interval;
        let showStyle = async function () {
          let style = this.fullStyle[n];
          if (!style) {
            return;
          }
          // 计算前 n 个 style 的字符总数
          let length = this.fullStyle
            .filter((_, index) => index <= n)
            .map((item) => item.length)
            .reduce((p, c) => p + c, 0);
          let prefixLength = length - style.length;
          if (this.currentStyle.length < length) {
            let l = this.currentStyle.length - prefixLength;
            let char = style.substring(l, l + 1) || " ";
            this.currentStyle += char;
            if (style.substring(l - 1, l) === "\n" && this.$refs.styleEditor) {
              this.$nextTick(() => {
                this.$refs.styleEditor.goBottom();
              });
            }
            setTimeout(showStyle, interval);
          } else {
            resolve();
          }
        }.bind(this);
        showStyle();
      });
    },
    progressivelyShowResume() {
      return new Promise((resolve, reject) => {
        let length = this.fullMarkdown.length;
        let interval = this.interval;
        let showResume = () => {
          if (this.currentMarkdown.length < length) {
            this.currentMarkdown = this.fullMarkdown.substring(
              0,
              this.currentMarkdown.length + 1
            );
            let lastChar = this.currentMarkdown[
              this.currentMarkdown.length - 1
            ];
            let prevChar = this.currentMarkdown[
              this.currentMarkdown.length - 2
            ];
            if (prevChar === "\n" && this.$refs.resumeEditor) {
              this.$nextTick(() => this.$refs.resumeEditor.goBottom());
            }
            setTimeout(showResume, interval);
          } else {
            resolve();
          }
        };
        showResume();
      });
    },
  },
};
</script>
<style scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

html {
  min-height: 100%;
}
.styleEditor {
  -webkit-backface-visibility: hidden;
}
</style>
