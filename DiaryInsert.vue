<template>
  <div class="content">
    <el-page-header>
      <template #content>
        <span class="text-large font-600 mr-3"> 다이어리 작성 </span>
      </template>
    </el-page-header>

    <div class="content">
      <div id="divNotice" class="notice-container">
        <p class="conTitle">
          <span>vue diary</span>
        </p>
        <!-- <el-button @click="goWrite">등록</el-button> -->
      </div>
    </div>
    <table>
      <tbody>
        <tr @click="focusInput">
          <th>제목</th>
          <td>
            <input
              ref="titleInput"
              type="text"
              id="title"
              v-model="title"
              placeholder="제목을 입력하세요"
            />
          </td>
        </tr>
        <tr>
          <th>날씨선택</th>
          <td class="vertical-align">
            <el-form-item prop="weather">
              <el-radio-group v-model="weather">
                <el-radio-button label="sunny">
                  <el-icon size="20"><Sunny /></el-icon>
                </el-radio-button>
                <el-radio-button label="cloudy">
                  <el-icon size="20"><Cloudy /></el-icon>
                </el-radio-button>
                <el-radio-button label="pouring">
                  <el-icon size="20"><Pouring /></el-icon>
                </el-radio-button>
                <el-radio-button label="lightning">
                  <el-icon size="20"><Lightning /></el-icon>
                </el-radio-button>
              </el-radio-group>
            </el-form-item>
          </td>
        </tr>
        <tr>
          <th>날짜선택</th>
          <td class="vertical-align">
            <div class="demo-date-picker">
              <div class="block">
                <el-date-picker
                  v-model="date"
                  type="date"
                  placeholder="날짜를 선택하세요."
                  :disabled-date="disabledDate"
                  :shortcuts="shortcuts"
                  :size="size"
                />
              </div>
            </div>
          </td>
        </tr>
        <tr>
          <th>기분</th>
          <td class="vertical-align mood-select">
            <select v-model="mood">
              <option value="1">매우 좋음</option>
              <option value="2">좋음</option>
              <option value="3">보통</option>
              <option value="4">우울</option>
              <option value="5">매우 나쁨</option>
            </select>
          </td>
        </tr>

        <tr>
          <th></th>
        </tr>
      </tbody>
    </table>

    <div id="divEditor">
      <quill-editor
        v-model:value="state.content"
        :options="state.editorOption"
        @change="onEditorChange($event)"
      />
    </div>
  </div>
</template>
<script>
import { quillEditor } from "vue3-quill";
import { reactive } from "vue";

export default {
  name: "App",
  components: {
    quillEditor,
  },
  data: function () {
    return {
      weather: "",
      date: "",
      title: "", // 제목
    };
  },

  setup() {
    const state = reactive({
      content: "",
      _content: "",
      editorOption: {
        placeholder: "내용을 작성해주세요.",
        modules: {
          toolbar: [
            ["bold", "italic", "underline", "strike"],
            ["blockquote", "code-block"],
            [{ header: 1 }, { header: 2 }],
            [{ list: "ordered" }, { list: "bullet" }],
            [{ script: "sub" }, { script: "super" }],
            [{ indent: "-1" }, { indent: "+1" }],
            [{ direction: "rtl" }],
            [{ size: ["small", false, "large", "huge"] }],
            [{ header: [1, 2, 3, 4, 5, 6, false] }],
            [{ color: [] }, { background: [] }],
            [{ font: [] }],
            [{ align: [] }],
            ["clean"],
            ["link", "image", "video"],
          ],
        },
        // more options
      },
      disabled: false,
    });

    const onEditorChange = ({ quill, html, text }) => {
      // 텍스트 편집기 내용이 변경될때 호출디는 콜백 함수 onEditorChange 정의
      state._content = html;
      console.log("############ onEditorChange START ############");
      console.log("quill          ::  ", quill);
      console.log("html           ::  ", html);
      console.log("text           ::  ", text);
      console.log("state._content ::  ", state._content);
      console.log("############ onEditorChange   END ############");
    };

    return {
      state,
      onEditorChange,
    };
  },
  //////////////////////////////methods////////////////////////////////////////////////////////////
  mounted() {
    this.focusInput();
  },

  methods: {
    focusInput() {
      if (this.$refs.titleInput) {
        this.$refs.titleInput.focus();
      }
    },
  },
};
</script>
<style>
tr,
th,
td {
  height: 70px;
  min-width: 108px;
  text-align: center;
}
#container ul li.contents .content {
  min-height: 450px;
}
#divEditor,
#divWeather {
  padding: 20px;
}
.ql-container {
  height: 200px;
}
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.box-card {
  width: 480px;
}
.list-main {
  margin-top: 2%;
}
.el-divider--horizontal {
  margin: 10px 0px 40px 0px;
}
.el-icon-edit {
  background-color: #9c97f0;
  color: white;
}
.el-icon-edit:hover {
  color: black;
}
.radio-group {
  margin-right: 20px;
}
.content input,
.content select {
  border: none;
}
.demonstration {
  color: var(--el-text-color-secondary);
}

.el-carousel__item h3 {
  color: #475669;
  opacity: 0.75;
  line-height: 150px;
  margin: 0;
  text-align: center;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
.el-page-header__header {
  font-family: "나눔바른고딕", NanumBarunGothic;
  line-height: 60px;
  font-size: 28px;
  font-weight: bold;
  margin: 0px 0px 20px;
}
div.content p.conTitle {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: auto !important;
}

table {
  width: 100%;
}
.vertical-align {
  vertical-align: middle;
}

/* 제목과 해당 입력 상자를 왼쪽에 정렬 */
th,
td {
  text-align: left;
}

/* 기분 select box를 오른쪽에 정렬 */
.mood-select {
  width: calc(100% - 170px); /* 남은 공간 계산 */
  display: inline-block; /* inline-block으로 변경 */
  vertical-align: middle; /* 수직 정렬 */
  margin-left: 50px; /* 왼쪽 마진 추가 */
}
</style>
