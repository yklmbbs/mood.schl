<template>
  <div>
    <marquee-text class="mb-3">
      <h1 class="color-color-primary">這裡是 YKLM 心情集散地，不論是告白 幹譙 靠北 傾訴 都歡迎來 PO 文～</h1>
    </marquee-text>

    <div class="row">
      <div class="col-12 col-md-6">
        <div class="form-group">
          <label class="col-label">內容編輯</label>
          <textarea-autosize
            class="form-control cards-editor"
            rows="7"
            minlength="30"
            maxlength="4096"
            placeholder="說吧～"
            required
            v-model="canvas.content"
            @keyup.native="onContentKeyup($event)"
          />
          <p class="text-danger text-right">
            <strong>【注意事項】字數有限制，字不能太少，也不能太多字。</strong>
          </p>
        </div>
        <!--form-group-->
      </div>
      <!--col-->

      <div class="col-12 col-md-6">
        <div class="form-group">
          <label class="col-label">預覽</label>
          <canvas id="previewCanvas" class="rounded mx-auto d-block w-100" width="960" height="720" ref="canvasView">
            <!-- 倘若使用者的瀏覽器並不支援 canvas，將會顯示該段內容。 -->
            您的瀏覽器必須支援 HTML5 標籤語法，才能使用圖片(即時)預覽功能。
          </canvas>
        </div>
        <!--form-group-->
      </div>
      <!--col-->
    </div>
    <!--row-->

    <div class="row">
      <div class="col-12 col-md-6">
        <div class="row">
          <div class="col-12">
            <div class="form-group">
              <label class="col-label">選擇樣式(文字、背景顏色)</label>
              <select
                class="form-control form-control-lg"
                :class="theme.options.find(option => option.value === theme.selector).class"
                v-model="theme.selector"
                @change="onThemeChange($event)"
              >
                <option
                  :class="option.class"
                  v-for="option in theme.options"
                  v-show="option.enable"
                  :key="option.value"
                  :value="option.value"
                >{{ option.text }}</option>
              </select>
            </div>
            <!--form-group-->
          </div>
          <!--col-->

          <div class="col-12">
            <div class="form-group">
              <label class="col-label">選擇字型(font)</label>
              <select
                class="form-control form-control-lg btn-dark text-white"
                v-model="font.selector"
                @change="onFontChange($event)"
              >
                <option
                  v-for="option in font.options"
                  :key="option.value"
                  :value="option.value"
                >{{ option.text }}</option>
              </select>
            </div>
            <!--input-group-->
          </div>
          <!--col-->
        </div>
        <!--row-->
      </div>
      <!--col-->

      <div class="col-12 col-md-6">
        <div class="form-group">
          <label class="col-label">發文類型</label>
          <select
            class="form-control form-control-lg btn-dark text-white"
            v-model="hashtag.selector"
            @change="onHashtagChange($event)"
          >
            <option
              v-for="option in hashtag.options"
              :key="option.value"
              :value="option.value"
            >{{ option.text }}</option>
          </select>
        </div>
        <!--form-group-->
        <div class="form-group d-none">
          <label class="col-label">To Be Continued</label>

          <div class="custom-control custom-checkbox">
            <input
              type="checkbox"
              id="to-be-continued"
              class="control-input"
              v-model="canvas.feature.is_to_be_continued"
            />
            <label
              class="color-color-primary control-label"
              for="to-be-continued"
            >是否在文章當中繪製 To Be Coutinued，建議樣式選擇「黑底白字」</label>
          </div>
        </div>
        <!--form-group-->
      </div>
      <!--col-->
    </div>
    <!--row-->

    <div class="row">
      <div class="col">
        <div class="form-group">
          <label class="col-label">自定義圖片上傳</label>
          <picture-input
            class="bg-black text-white"
            buttonClass="h3 btn btn-block btn-dos btn-lg"
            width="1920"
            height="360"
            margin="12"
            size="2"
            accept="image/jpeg, image/png, image/gif"
            :zIndex="0"
            :crop="false"
            ref="avatarInput"
            :customStrings="{
                            drag: '點我可以直接上傳圖片ㄛ🐱',
                            change: '換別張圖好惹',
                        }"
            @change="onAvatarChange"
          ></picture-input>
        </div>
        <!--input-group-->
      </div>
      <!--col-->
    </div>
    <!--row-->

    <div class="row">
      <div class="col">
        <div class="jumbotron jumbotron-fluid p-2 rounded">
          <div class="container">
            <h1 class="text-center py-2">板規</h1>
            <pre>
一、責任聲明：
本站使用者須對自己所張貼之每一篇文章負責，本站毋需對站內以及其他關聯之社群媒體的言論負擔起任何的責任，責任的歸屬權屬於各位發表人。

二、發表文章時之注意事項：
1. 尊重他人意見，注意用字遣詞與口氣，避免引起爭吵。
2. 避免在公眾區域，討論私人事務。
3. 避免發表文章於非相關區域，文章標題及內容不符合討論區之討論主題。
4. 禁止重複刊登相同內容或相同意義之留言。
5. 適度引用文章，避免引用過長文章，造成閱讀困擾。
6. 不適當的廣告、宣傳活動或商業性留言。
7. 禁止發表謾罵、脅迫、挑釁、猥褻或不雅之文字。
8. 禁止發表個人測試用文章或散播不實消息之文章，張貼文章，應自負相關法律責任。
9. 轉貼任何文章請附上原作者貨來源，否則版主會親自去問授權、處理方式，並且把您的帳號封鎖。
10. 若有未規定的部份，由版主依主觀認定，視情況處理。

三、違規處理辦法：
違反上述規定之文章或作者，版主可刪除文章或行使禁貼之處份。

四、附註及補充說明：
1. 本站歡迎網友互相討論發表己見，唯請務必遵守上述規定。
2. 是否違反上述規定，由版主主觀認定，請謹慎用詞。
3. 請學習包容各種意見，如遇惡意批評或攻擊之文章，切勿加入爭執，並且善用檢舉，版主會有適當之處理，否則雙方皆依上述規定處理。
                        </pre>
            <hr />
            <div class="pretty p-icon p-toggle p-plain">
              <input type="checkbox" id="checkbox" v-model="checked" />
              <div class="state p-on p-info-o">
                <i class="icon bi bi-heart-fill"></i>
                <label class="text-info">我真的有看，且願意遵守以上的板規</label>
              </div>
              <div class="state p-off">
                <i class="icon bi bi-heart"></i>
                <label class="text-danger">我不想遵守板規</label>
              </div>
            </div>

          </div>
        </div>
      </div>
      <!--col-->
    </div>
    <!--row-->

    <div class="row" v-if="this.isAdmin">
      <div class="col">
        <div class="form-group">
          <label class="col-label">版主識別框線</label>

          <div class="custom-control custom-checkbox">
            <input
              type="checkbox"
              id="manager-line"
              class="control-input"
              v-model="canvas.is_manager_line"
            />
            <label class="color-color-primary control-label" for="manager-line">是否在文章當中繪製版主群識別框線</label>
          </div>
        </div>
        <!--form-group-->
      </div>
      <!--col-->
    </div>
    <!--row-->

    <div class="row">
      <div class="col">
        <div class="form-group clearfix">
          <label class="col-label">將文章發表出去</label>
          <button class="h3 btn btn-block btn-dos btn-lg" @click="publish">發表文章</button>
        </div>
        <!--form-group-->
      </div>
      <!--col-->
    </div>
    <!--row-->

    <audio ref="audio" src="/music/種子.mp3"></audio>
  </div>
  <!--container-->
</template>

<script>
import MarqueeText from "vue-marquee-text-component";
import PictureInput from "vue-picture-input";
import FontFaceObserver from "fontfaceobserver";
import { required, minLength } from "vuelidate/lib/validators";

export default {
  name: "SocialCardsCreate",
  components: {
    MarqueeText,
    PictureInput
  },
  props: {
    isAdmin: {
      type: Number,
      required: false,
      default: false
    }
  },
  data() {
    return {
      qrcodeImg: null,
      checked: false,
      canvas: {
        view: null,
        ctx: null,
        default_width: 960,
        default_height: 720,
        width: 960,
        height: 720,
        is_center: true,
        content: null,
        color: "#F8F9FA",
        background_color: "#E83E8C",
        font: "Auraka",
        is_manager_line: this.isAdmin,
        feature: {
          is_to_be_continued: false
        },
        hashtag: "港覺"
      },
      avatar: null,
      theme: {
        selector: "7d37ef838c73b3397403eec4bf4f3839",
        options: [
          {
            text: "黑底綠字",
            class: "bg-dark text-success",
            value: "2e6046c7387d8fbe9acd700394a3add3",
            enable: true,
            color: {
              background: "#000000",
              text: "#00FF3B"
            }
          },
          {
            text: "黑底黃字",
            class: "bg-dark text-warning",
            value: "be551aa525b9d13790278b008a9ec7bf",
            enable: true,
            color: {
              background: "#000000",
              text: "#EBD443"
            }
          },
          {
            text: "黑底白字",
            class: "bg-dark text-white",
            value: "8a755c0bd32e29f813c1aa4267357d5a",
            enable: true,
            color: {
              background: "#000000",
              text: "#F8F9FA"
            }
          },
          {
            text: "黑底紅字",
            class: "bg-dark text-danger",
            value: "507d8c23bdcc98850c7be1c1286d5dcf",
            enable: true,
            color: {
              background: "#000000",
              text: "#DC3545"
            }
          },
          {
            text: "甜甜香草巧克力熊貓",
            class: "bg-pink text-white",
            value: "7d37ef838c73b3397403eec4bf4f3839",
            enable: true,
            color: {
              background: "#E83E8C",
              text: "#F8F9FA"
            }
          },
          {
            text: "藍白屏",
            class: "bg-primary text-white",
            value: "4834578267bcb800feb2762d2a3ccff2",
            enable: true,
            color: {
              background: "#007BFF",
              text: "#F8F9FA"
            }
          },
          {
            text: "PostgreSQL",
            class: "bg-light text-primary",
            value: "dc7b1c2c41639e5cf10f725d60ad8c64",
            enable: true,
            color: {
              background: "#F8F9FA",
              text: "#007BFF"
            }
          },
          {
            text: "Laravel",
            class: "bg-laravel text-white",
            value: "a5c95b86291ea299fcbe64458ed12702",
            enable: true,
            color: {
              background: "#F4645F",
              text: "#F8F9FA"
            }
          },
          {
            text: "軟體綠",
            class: "bg-softgreen text-dark",
            value: "731019ad725385614d65fbcc5fb1758e",
            enable: true,
            color: {
              background: "#39C5BB",
              text: "#000000"
            }
          },
          {
            text: "皮卡丘",
            class: "bg-switch color-pikachu",
            value: "9CE44F88A25272B6D9CBB430EBBCFCF1",
            enable: true,
            color: {
              background: "#2F3437",
              text: "#FFD547"
            }
          },
          {
            text: "伊布",
            class: "bg-switch color-eevee",
            value: "640ED62B7D35C1765A05EB8724535A53",
            enable: true,
            color: {
              background: "#2F3437",
              text: "#E7AF56"
            }
          },
          {
            text: "反向 皮卡丘",
            class: "bg-pikachu color-switch",
            value: "9A2E33D968A1AF98B09E26AC63CB6DCB",
            enable: true,
            color: {
              background: "#FFD547",
              text: "#2F3437"
            }
          },
          {
            text: "反向 伊布",
            class: "bg-eevee color-switch",
            value: "98C614FBC16CCF5D5740BD4D4E00757C",
            enable: true,
            color: {
              background: "#E7AF56",
              text: "#2F3437"
            }
          },
          {
            text: "新年限定主題",
            class: "bg-ch-new-year-2019-red color-ch-new-year-2019-yellow",
            value: "2be6c9a365a26a12876145e9229639b1",
            enable: true,
            color: {
              background: "#A61723",
              text: "#D8B06A"
            }
          },
          {
            text: "反向 新年限定主題",
            class: "bg-ch-new-year-2019-yellow color-ch-new-year-2019-red",
            value: "b9b8ae80a601616cb9af07aaabe532f4",
            enable: true,
            color: {
              background: "#D8B06A",
              text: "#A61723"
            }
          },
          {
            text: "粉紅色",
            class: "bg-pink-secondary color-pink",
            value: "j874kwoxi2nh64yt67wtphy9m5dmea4q",
            enable: true,
            color: {
              background: "#F8C0C8",
              text: "#FF5376"
            }
          },
          {
            text: "Windows 最棒的畫面",
            class: "bg-windows-10-error text-white",
            value: "32d2a897602ef652ed8e15d66128aa74",
            enable: true,
            color: {
              background: "#007BD0",
              text: "#F8F9FA"
            }
          },
          {
            text: "Windows 最棒的畫面 測試人員組件",
            class: "bg-windows-10-testing-error text-white",
            value: "tumx453xqZLjf5kaFFBzNj4gqVXKWqXz",
            enable: true,
            color: {
              background: "#107C10",
              text: "#F8F9FA"
            }
          },
          {
            text: "恭迎慈孤觀音 渡世靈顯四方",
            class: "bg-devotion text-dark",
            value: "05217b7d4741e38096a54eff4226c217",
            enable: false,
            color: {
              background: "#F11541",
              text: "#000000"
            }
          },
          {
            text: "支離滅裂な思考・発言",
            class: "bg-light text-dark",
            value: "05326525f82b9a036e1bcb53a392ff7c",
            enable: false,
            color: {
              background: "#F8F9FA",
              text: "#000000"
            }
          }
        ]
      },
      hashtag: {
        selector: "#feeling",
        options: [
          {
            text: "八卦",
            value: "#gossip",
            board: "Chat"
          },
          {
            text: "打屁",
            value: "#chat",
            board: "Chat"
          },
          {
            text: "告白",
            value: "#confess",
            board: "Church"
          },
          {
            text: "靠北",
            value: "#grumble",
            board: "grumble"
          },
          {
            text: "港覺",
            value: "#feeling",
            board: "Feeling"
          },
          {
            text: "許願",
            value: "#wishes",
            board: "Church"
          },
        ]
      },
      font: {
        selector: "ea98dde8987df3cd8aef75479019b688",
        options: [
          {
            text: "AURAKA 點陣宋字型",
            font: "Auraka",
            value: "ea98dde8987df3cd8aef75479019b688"
          },
          {
            text: "國喬點陣字型",
            font: "KC24M",
            value: "813ca6cbbd95d7e08fa2af59bc12072d"
          },
          {
            text: "Sliver 點陣字型",
            font: "Silver",
            value: "27B9CEBCC87F3E1FAC42DA21B38F08C0"
          },
          {
            text: "ZPIX 點陣字型",
            font: "ZPIX",
            value: "1b23b3cd9223930ac694b7f29f38ff21"
          },
          {
            text: "蒙納繁圓點陣",
            font: "MBitmapRoundHK",
            value: "f762e3a99692b40e5929ab3668606a4a"
          },
          {
            text: "Cheek 高中少女體",
            font: "CheekFont",
            value: "5b50cf3b30de1a46b112899f16f6e4bf"
          },
          {
            text: "清松手寫體",
            font: "JasonHandwriting1",
            value: "b2b69f4f0055b4a20fd55a650acc60d9"
          },
          {
            text: "CJKFonts手寫4",
            font: "CJKFonts Handingwriting4",
            value: "b4d1b9025e1866296299e4aa7dba4639"
          },
          {
            text: "CJKFonts全瀨體",
            font: "CJKFonts Allseto",
            value: "6423dac3a45b20e5f01609175de3a291"
          },
          {
            text: "和田研細丸ゴシック",
            font: "Timemachine Wa",
            value: "6ACA4ADF916F8D5419E17654D1B3AD80"
          },
          {
            text: "微軟正黑體",
            font: "Microsoft JhengHei",
            value: "13f5333afe00f8c7e8da7e0b13ec2c94"
          },
          {
            text: "標楷體",
            font: "Kaiu",
            value: "21881fc6a87aca0dd1afc685cb6ee891"
          },
          {
            text: "新細明體",
            font: "Mingliu",
            value: "c0b5dd764ede0ca105be22cf13ebadff"
          },
          {
            text: "張海山銳諧體",
            font: "Harmonic",
            value: "68068fcf50e7cae709cb8ed0b7b9b0f3"
          },
          {
            text: "裝甲明朝體",
            font: "SoukouMincho",
            value: "77169c474432f72879856e6d771b3129"
          },
          {
            text: "極粗明朝體",
            font: "MatissePro EB",
            value: "ozke4ri3gkpy7e9c312u5l0w5vr9jdqq"
          },
          {
            text: "台北黑體",
            font: "Taipei Sans TC Beta",
            value: "yc45sgsfbss490dqgs2g23a7z24slhoj"
          }
        ]
      }
    };
  },
  validations: {
    canvas: {
      content: {
        required,
        minLength: minLength(6)
      }
    },
    theme: {
      selector: {
        required
      }
    },
    font: {
      selector: {
        required
      }
    },
    hashtag: {
      selector: {
        required
      }
    }
  },
  methods: {
    async getQRCode() {
      axios
        .get("/api/frontend/social/cards/token/qrcode")
        .then((response) => {
          this.qrcodeImg = response.data;
          console.log(response);
          return response.data;
        })
        .catch((error) => console.log(error));
    },
    onAvatarChange(avatar) {
      console.log("New picture selected!");
      if (avatar) {
        console.log("Picture loaded.");
        this.avatar = this.$refs.avatarInput.file;
      } else {
        console.log("FileReader API not supported: use the <form>, Luke!");
      }
    },
    onContentKeyup(event) {
      this.canvas.content = event.target.value;
      this.drawingAll();
    },
    onThemeChange(event) {
      const theme = this.theme.options.find(
        option => option.value === this.theme.selector
      );
      this.canvas.color = theme.color.text;
      this.canvas.background_color = theme.color.background;

      this.drawingAll();
    },
    onHashtagChange(event) {
      const hashtag = this.hashtag.options.find(
        option => option.value === this.hashtag.selector
      );
      this.canvas.hashtag = hashtag.text;
      this.drawingAll();
    },
    onFontChange(event) {
      const font = this.font.options.find(
        option => option.value === this.font.selector
      );
      const ffo = new FontFaceObserver(font.font);
      ffo.load(null, 9000).then(
        function() {
          console.log(font.font + " is loaded.");
          this.drawingAll();
        }.bind(this),
        function() {
          Swal.fire({
            title: "字體需要一點時間載入，載入完成後將會自動替換字體。",
            width: 600,
            padding: "3em",
            backdrop: `
                            rgba(0, 0, 0, 0.4)
                            url('/img/frontend/gif/nyan-cat.gif')
                            center left
                            no-repeat
                        `
          });
        }
      );
      this.canvas.font = font.font;
    },
    drawingAll() {
      this.resetCanvasView();

      this.settingCanvasViewSize();
      this.drawingBackground();
      this.drawingManagerLine();
      this.drawingBackgroundImage();
      this.drawingLogo();
      this.drawingUrl();
      this.drawingContent();
      this.drawingFeature();
    },
    resetCanvasView() {
      this.canvas.view = this.$refs.canvasView;
      this.canvas.ctx = this.canvas.view.getContext("2d");
    },
    settingCanvasViewSize() {
      let lineCount = this.contentSplit().length;
      let canvasView_center = lineCount * 80 < 600 ? true : false;
      let canvasView_height = canvasView_center
        ? this.canvas.default_height
        : 72 + 72 + lineCount * 80;
      let canvasView_width = this.canvas.default_width;

      /**
       * 特殊樣式重新賦予長寬
       */
      switch (this.theme.selector) {
        case "32d2a897602ef652ed8e15d66128aa74":
          canvasView_height += 360;
          break;

        case "tumx453xqZLjf5kaFFBzNj4gqVXKWqXz":
          canvasView_height += 360;
          break;

        case "05326525f82b9a036e1bcb53a392ff7c":
          canvasView_height += 140;
          canvasView_width += 349;
          break;
      }

      /**
       * Feature
       */
      if (this.canvas.feature.is_to_be_continued) canvasView_height += 160;

      this.canvas.is_center = canvasView_center;
      this.canvas.view.width = canvasView_width;
      this.canvas.view.height = canvasView_height;
      this.canvas.width = canvasView_width;
      this.canvas.height = canvasView_height;
    },
    drawingBackground() {
      this.canvas.ctx.fillStyle = this.canvas.background_color;
      this.canvas.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height);
    },
    drawingFeature() {
      let img = new Image();
      if (this.canvas.feature.is_to_be_continued) {
        img.src = "/img/frontend/cards/to_be_continued.png";
        img.onload = function () {
          this.canvas.ctx.drawImage(img, 24, this.canvas.view.height - 240);
        }.bind(this);
      }
    },
    drawingBackgroundImage() {
      let img = new Image();
      switch (this.theme.selector) {
        case "05217b7d4741e38096a54eff4226c217":
          img.src = "/img/frontend/cards/devotion-bg.png";
          img.onload = function () {
            this.canvas.ctx.drawImage(img, 360, 64);
          }.bind(this);
          return;

        case "32d2a897602ef652ed8e15d66128aa74":
        case "tumx453xqZLjf5kaFFBzNj4gqVXKWqXz":
          // img.src = "/img/frontend/cards/qrcode.png";
          img.src = this.qrcodeImg;
          img.onload = function () {
            this.canvas.ctx.drawImage(img, 24, this.canvas.height - 204);
          }.bind(this);
          return;

        // case "05326525f82b9a036e1bcb53a392ff7c":
        //   img.src = "/img/frontend/cards/fragmented_background.png";
        //   img.onload = function () {
        //     self.canvas.ctx.drawImage(img, 0, self.canvas.height - 560);
        //   }
        //   img.src = "/img/frontend/cards/fragmented_people.png";
        //   img.onload = function () {
        //     self.canvas.ctx.drawImage(img, 36, self.canvas.height - 542);
        //   }
        //   this.canvas.ctx.lineJoin = "round";
        //   this.canvas.ctx.lineWidth = 8;
        //   this.canvas.ctx.strokeRect(
        //     353,
        //     40,
        //     this.canvas.width - 381,
        //     this.canvas.height - 282
        //   );
        //   this.canvas.ctx.fillRect(
        //     357,
        //     44,
        //     this.canvas.width - 389,
        //     this.canvas.height - 290
        //   );
        //   img.src = "/img/frontend/cards/fragmented_background_arrow.png";
        //   // img.src = "https://i.imgur.com/8kMDGcA.png";
        //   img.onload = function () {
        //     self.canvas.ctx.drawImage(img, 312, self.canvas.height - 388);
        //   }
        //   return;
      }
    },
    drawingLogo() {
      switch (this.theme.selector) {
        case "32d2a897602ef652ed8e15d66128aa74":
        case "tumx453xqZLjf5kaFFBzNj4gqVXKWqXz":
          this.canvas.ctx.font = "36px " + this.canvas.font;
          this.canvas.ctx.fillStyle = this.canvas.color;
          this.canvas.ctx.fillText(
            "若要深入了解，您稍候可以線上搜尋此:",
            228,
            this.canvas.height - 160
          );
          this.canvas.ctx.fillText(
            "情緒泥巴 0xMOODS_YKLM " + this.hashtag.selector,
            228,
            this.canvas.height - 120
          );
          this.canvas.ctx.fillText(
            "請訪問 " + process.env.MIX_APP_URL,
            228,
            this.canvas.height - 40
          );
          return;

        // case "05326525f82b9a036e1bcb53a392ff7c":
        //   this.canvas.ctx.font = "72px " + this.canvas.font;
        //   this.canvas.ctx.fillStyle = this.canvas.color;
        //   this.canvas.ctx.fillText("支離滅裂な", 360, this.canvas.height - 160);
        //   this.canvas.ctx.fillText("思考・発言", 360, this.canvas.height - 80);
        //   this.canvas.ctx.font = "36px " + this.canvas.font;
        //   this.canvas.ctx.fillStyle = this.canvas.color;
        //   this.canvas.ctx.fillText(
        //     process.env.MIX_APP_NAME,
        //     this.canvas.width - 232,
        //     this.canvas.height - 24
        //   );
        //   return;

        default:
          this.canvas.ctx.font = "36px " + this.canvas.font;
          this.canvas.ctx.fillStyle = this.canvas.color;
          this.canvas.ctx.fillText(
            this.canvas.hashtag + process.env.MIX_APP_NAME,
            this.canvas.width - 160,
            this.canvas.height - 24
          );
          return;
      }
    },
    drawingUrl() {
      switch (this.theme.selector) {
        case "32d2a897602ef652ed8e15d66128aa74":
        case "tumx453xqZLjf5kaFFBzNj4gqVXKWqXz":
          this.canvas.ctx.font = "192px " + this.canvas.font;
          this.canvas.ctx.fillStyle = this.canvas.color;
          this.canvas.ctx.fillText(":(", 48, 192);
          return;

        default:
          this.canvas.ctx.font = "36px " + this.canvas.font;
          this.canvas.ctx.fillStyle = this.canvas.color;
          this.canvas.ctx.fillText(
            this.hashtag.selector,
            16,
            this.canvas.height - 65
          );
          this.canvas.ctx.fillText(
            process.env.MIX_APP_TITLE + " " + process.env.MIX_APP_URL,
            16,
            this.canvas.height - 24
          );
          return;
      }
    },
    drawingContent() {
      let contentList = this.contentSplit();
      contentList.forEach(
        function(content_value, content_key) {
          let x_point = 36;
          let y_point = 0;
          if (this.canvas.is_center) {
            y_point =
              24 + this.canvas.is_center
                ? 440 + ((content_key - 1) * 80 - contentList.length * 40)
                : (content_key + 1) * 80;
          } else {
            y_point = 96 + content_key * 80;
          }

          switch (this.theme.selector) {
            case "32d2a897602ef652ed8e15d66128aa74":
            case "tumx453xqZLjf5kaFFBzNj4gqVXKWqXz":
              y_point += 240;
              break;
            case "05326525f82b9a036e1bcb53a392ff7c":
              x_point += 349;
              y_point += 24;
              break;
          }

          this.canvas.ctx.font = "63px " + this.canvas.font;
          this.canvas.ctx.fillStyle = this.canvas.color;
          this.canvas.ctx.fillText(content_value, x_point, y_point);
        }.bind(this)
      );
    },
    drawingManagerLine() {
      if (this.canvas.is_manager_line) {
        for (let i = 6; i < 12; i++) {
          let rectangle = new Path2D();
          rectangle.rect(
            i,
            i,
            this.canvas.width - i * 2,
            this.canvas.height - i * 2
          );
          this.canvas.ctx.strokeStyle = this.canvas.color;
          this.canvas.ctx.stroke(rectangle);
        }
      }
    },
    contentSplit() {
      let content = this.canvas.content !== null ? this.canvas.content : "";
      let response_list = [];
      let content_list = content.split(/\r\n|\r|\n/);
      content_list.forEach(function(content_value) {
        let content_strlen = encodeURIComponent(content_value).replace(
          /%[A-F\d]{2}/g,
          "U"
        ).length;
        if (content_strlen <= 42) {
          response_list.push(content_value);
        } else {
          let content_width = 0;
          let char_string = "";
          let _content_value_list = content_value.split("");
          _content_value_list.forEach(function(char_value, char_key) {
            let char_strlen = encodeURIComponent(char_value).replace(
              /%[A-F\d]{2}/g,
              "U"
            ).length;
            content_width += char_strlen == 3 ? 1 : 0.5;
            char_string += char_value;
            if (char_key + 1 in _content_value_list) {
              let _next_char_strlen = encodeURIComponent(
                _content_value_list[char_key + 1]
              ).replace(/%[A-F\d]{2}/g, "U").length;
              let _next_char_width = _next_char_strlen == 3 ? 1 : 0.5;
              if (content_width + _next_char_width >= 14) {
                response_list.push(char_string);
                content_width = 0;
                char_string = "";
              }
            }
          });

          if (char_string != "") {
            response_list.push(char_string);
          }
        }
      });

      return response_list;
    },
    publish() {
      this.$v.$touch();
      if (!this.checked) {
        Swal.fire("哦噢 ...", "不想遵守板規就沒辦法幫你發文囉。", "error");
        return;
      }

      if (this.$v.$invalid) {
        Swal.fire(
          "您根本的內容不符合規範啊！",
          "我對於你們在學校所受的訓練為什麼會是這個樣子，我深感不解。",
          "error"
        );
        return;
      }

      Swal.fire({
        title: "您確定要發表文章嗎？",
        text: "如果您按下射射射，那文章就真的會射出去了。",
        showCancelButton: true,
        showLoaderOnConfirm: true,
        confirmButtonColor: "#3085d6",
        confirmButtonText: "射射射",
        cancelButtonColor: "#d33",
        cancelButtonText: "不要！",
        allowOutsideClick: () => !Swal.isLoading(),
        preConfirm: login => {
          let data;
          if (this.avatar) {
            data = new FormData();
            data.append("content", this.canvas.content);
            data.append("hashtag", this.hashtag.selector);
            data.append("themeStyle", this.theme.selector);
            data.append("fontStyle", this.font.selector);
            data.append("avatar", this.avatar);
          } else {
            var finalCanvas = document.getElementById('previewCanvas');
            var canvasBase64 = finalCanvas.toDataURL();
            data = {
              content: this.canvas.content,
              hashtag: this.hashtag.selector,
              themeStyle: this.theme.selector,
              fontStyle: this.font.selector,
              isManagerLine: this.canvas.is_manager_line,
              isFeatureToBeCoutinued: this.canvas.feature.is_to_be_continued,
              base64: canvasBase64,
            };
          }

          return axios
            .post(
              this.isAdmin
                ? "/api/backend/social/cards/token/publish"
                : "/api/frontend/social/cards/token/publish",
              data
            )
            .then(function(response) {
              return response;
            })
            .catch(function(error) {
              return error;
            });
        }
      }).then(result => {
        switch (result.value.status) {
          case 200:
            this.$refs.audio.play();

            let timerInterval;
            Swal.fire({
              title: "射射射！",
              html: `文章射出去惹，系統將在 <b></b> 毫秒後自動前往。<br>或者<a href="/cards/show/${result.value.data.data.id}" class="btn btn-rainbow p-1 m-1">按我</a>直接前往。`,
              timer: 5000,
              timerProgressBar: true,
              allowOutsideClick: false,
              onBeforeOpen: () => {
                Swal.showLoading();
                timerInterval = setInterval(() => {
                  Swal.getContent().querySelector(
                    "b"
                  ).textContent = Swal.getTimerLeft();
                }, 100);
              },
              onClose: () => {
                clearInterval(timerInterval);
                window.location.href = `/cards/show/${result.value.data.data.id}`;
              }
            }).then(result => {
              if (result.dismiss === Swal.DismissReason.timer) {
                console.log("I was closed by the timer");
              }
            });
            break;

          default:
            Swal.fire(
              "啊 ... 卡住了。",
              "文章並沒有被射出去，建議您去問問作者花生神魔術惹？",
              "error"
            );
            break;
        }
      });
    }
  },
  created () {
    this.getQRCode();
  },
  mounted() {
    this.drawingAll();
  },
  watch: {
    'canvas.is_manager_line': function(val) {
      this.drawingAll();
    },
    'canvas.feature.is_to_be_continued': function(val) {
      this.drawingAll();
    },
    'canvas.font': function(val) {
      this.drawingAll();
    },
  }
};
</script>
