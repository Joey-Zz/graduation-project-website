<template>
  <div class="home">
    <Login v-if="showLogin" @close="showLogin=false" />
    <template v-if="user._id">
      <div class="main">
        <div class="revise">
          <div class="revise-left">
            <div class="revise-num">
              {{reviseNum}}
              <div id="revise" @click="goRevise">待复习</div>
            </div>
          </div>

          <div class="revise-right">
            <div class="revise-title"></div>
            <div class="learn-list" @click="goLearn('list1')">
              <div class="list-title">待背单词</div>
              <div class="list-hint">共300个单词</div>
              <div class="list-more">{{ getListHint('list1') }}</div>
              <div class="list-hover"></div>
            </div>
          </div>
        </div>
      </div>
    </template>
    <div v-else class="main">
      
    </div>
  </div>
</template>

<script>
import Login from "@/components/Login";
import word from "@/api/word";

export default {
  name: "home",
  components: {
    Login
  },
  mounted() {
    this.$event.on("login", this, () => {
      this.showLogin = true;
    });
    setTimeout(() => {
      this.getListProgress();
      this.getReviseNum();
    }, 200);
  },
  data() {
    return {
      showLogin: false,
      reviseNum: 0,
      listProgress: {}
    };
  },
  computed: {
    user() {
      return this.$store.state.user;
    }
  },
  methods: {
    getListProgress() {
      word
        .getUserProgress()
        .then(progress => {
          this.listProgress = progress || {};
          console.log(progress);
        })
        .catch(err => console.log(err));
    },
    getReviseNum() {
      word
        .getReviseWordNum()
        .then(num => {
          this.reviseNum = num;
        })
        .catch(err => console.log(err));
    },
    getListHint(listName) {
      const progress = this.listProgress[listName];
      const wordNum = word.getListWordNum(listName);
      if (progress) {
        const learnedNum = progress.location;
        if (learnedNum >= wordNum) return "已完成学习";
        return `已学习${learnedNum}词`;
      } else {
        return `未开始`;
      }
    },
    goRevise() {
      this.$router.push("/revise");
    },
    goLearn(listName) {
      this.$router.push(`/learn?list=${listName}`);
    }
  }
};
</script>

<style scoped>
.home {
  width: 100vw;
  height: 100vh;
  background: #f0f0f0;
  overflow-x: hidden;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
}

.main {
  height: fit-content;
  padding: 100px;
  box-sizing: border-box;
  max-width: 1000px;
  margin: 5% 5% 5% 5%;
}

#revise {
  cursor: pointer;
}

.revise {
 
  background: #ffffff;
  box-shadow: 0 2px 7px 0 rgba(0, 0, 0, 0.24);
  border-radius: 4px;
  padding: 35px;
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  align-items: center;
}

.revise-left {
  width: 140px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.revise-num {
  width: 115px;
  height: 115px;
  border-radius: 50%;
  border: 9px solid #c1cace;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  font-size: 20px;
  color: #015c92;
  font-weight: 600;
  border: 8px solid #c1cace;
}

.revise-num span {
  margin-top: 6px;
  font-size: 15px;
  color: #455358;
  font-weight: 700;
}

.revise-right {
  width: 100%;
  height: 100%;
  padding-left: 35px;
  box-sizing: border-box;
  
}

.revise-title {
  width: 100%;
  font-size: 36px;
  font-weight: 700;
  color: #455358;
  text-align: left;
}

.revise-hint {
  width: 100%;
  color: #455358;
  font-size: 17px;
  margin-top: 15px;
  text-align: left;
}

.hint {
  width: 100%;
  font-weight: 600;
  font-size: 18px;
  color: #455358;
  margin-top: 30px;
  text-align: left;
}

.learn {
  width: 80%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin-top: 15px;
}

.learn-list {
  width: 80%;
  border-radius: 5px;
  box-shadow: 0 0 1.5px 0 rgba(0, 0, 0, 0.24);
  background: #ffffff;
  padding: 20px 20px 20px 20px;
  box-sizing: border-box;
  margin-bottom: 20px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.list-hover {
  width: 100%;
  height: 5px;
  background: #015c92;
  opacity: 0;
  transition: all 0.4s;
  position: absolute;
  bottom: 0;
  left: 0;
}

.learn-list:hover .list-hover {
  opacity: 1;
}

.list-title {
  width: 100%;
  font-size: 21px;
  font-weight: 700;
  color: #455358;
  text-align: left;
}

.list-hint {
  width: 100%;
  color: #99a5aa;
  font-size: 14px;
  margin-top: 8px;
  text-align: left;
  font-weight: 700;
}

.list-more {
  width: 100%;
  color: #455358;
  font-size: 13px;
  margin-top: 35px;
  text-align: left;
  font-weight: 600;
}

.header {
  width: 100%;
  color: #015c92;
  text-align: center;
  font-size: 42px;
  letter-spacing: 3px;
  font-weight: bold;
  margin-top: 40px;
  margin-bottom: 80px;
}

.start {
  width: 240px;
  height: 64px;
  margin: 180px auto 0 auto;
  background: #2d82b5;
  color: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
  font-weight: 600;
  letter-spacing: 2px;
  text-decoration: none;
  transition: all 0.5s ease;
  cursor: pointer;
}

.start:hover {
  background: #015c92;
}

.info {
  position: absolute;
  bottom: 0;
  height: 80px;
  width: 100%;
  background: #ffffff;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 15px;
  font-weight: 500;
  color: #999999;
  line-height: 30px;
  cursor: pointer;
}

.info span {
  font-size: 14px;
}
</style>
