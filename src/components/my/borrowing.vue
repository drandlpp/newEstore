<template>
  <div class="wrapper">
    <div class="moduleHead">
      <a href="javascript:window.history.go(-1)"></a>借贷
    </div>
    <div class="borrowing-container">
      <div class="borrowing-head">
        <h1 class="borrowing-head-title">借款金额</h1>
        <div class="borrowing-head-chose">
          <p><input type="radio" id="v1" name="borrow-num" value="1" v-model="borrowInfo.money"><label for="v1">30000</label></p>
          <!--<p><input type="radio" id="v2" name="borrow-num" value="2" v-model="borrowInfo.money"><label for="v2">60000</label></p>-->
          <!--<p><input type="radio" id="v3" name="borrow-num" value="3" v-model="borrowInfo.money"><label for="v3">90000</label></p>-->
        </div>
      </div>
      <div class="borrowing-info">
        <h1 class="borrowing-head-title">个人信息</h1>
        <div class="borrowing-info-center">
          <router-link to="/bankManage/1">
            <div class="borrowing-info-list">
              <span class="borrowing-info-title">银行账户</span>
              <input type="text" placeholder="请选择到账账户" readonly disabled v-model="bankAccount">
            </div>
          </router-link>
          <div class="borrowing-info-list">
            <span class="borrowing-info-title">真实姓名</span>
            <input type="text" placeholder="请输入真实姓名" v-model="borrowInfo.realName">
          </div>
          <div class="borrowing-info-list">
            <span class="borrowing-info-title">身份证号</span>
            <input type="text" placeholder="请输入身份证号" v-model="borrowInfo.idNum">
          </div>
          <div class="borrowing-info-list">
            <span class="borrowing-info-title">预留电话</span>
            <input type="number" placeholder="请输入银行预留电话" v-model="borrowInfo.tel">
          </div>
          <div class="borrowing-info-pto">
            <p>手持身份证半身照</p>
            <div class="bit-center">
              <img :src="borrowInfo.imgs" alt="" v-if="borrowInfo.isUp">
              <img src="../../assets/images/upp.jpg" alt="" v-if="!borrowInfo.isUp">
              <input type="file"  ref="inputer"  accept="image/*" @change = 'addImg'>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="borrowing-agreement">
      <input type="checkbox" id="agreement" v-model="borrowInfo.agree" @click="ag">
      <label for="agreement">我已经阅读并同意 </label><span @click="readAgreement(0)">《用户借款协议》</span>
    </div>
    <div class="borrowing-btn" @click="borrowMoney" :class="{'borrowing-btn-c': !isC}">{{btnTxt}}</div>
    <div class="agreement-container" :style="aClass">
      <div class="agreement-center">
        <h4>苏格优品商城用户注册协议（以下简称本协议）由南阳苏格实业有限公司（以下简称本公司）和您签订。</h4>
        <h5>一、声明与承诺</h5>
        <p>（一）在接受本协议或您以本公司允许的其他方式实际使用苏格优品商城（以下简称苏格优品商城）之前，
          请您仔细阅读本协议的全部内容。如果您不同意本协议的任意内容，或者无法准确理解本公司对条款的解释，
          请不要进行后续操作，包括但不限于不要接受本协议，不使用本服务。如果您对本协议的条款有疑问，请通过本公
          司客服渠道进行询问（咨询电话为4006203777），本公司将向您解释条款内容。</p>
        <p>（二）您同意，如本公司需要对本协议进行变更或修改的，须提前予以公布，公告期限届满后即时生效；若您在本协议内容公告变更生效后继续使用苏格优品商城的，表示您已充分阅读、理解并接受变更后的协议内容，也将遵循变更后的协议内容使用苏格优品商城；若您不同意变更后的协议内容，您应在变更生效前停止使用苏格优品商城。</p>
        <p>（三）如您为无民事行为能力人或为限制民事行为能力人，例如您未满18周岁，则您应在监护人监护、指导下阅读本协议和使用本服务。若您非自然人，则您确认，在您取得苏格优品商城账户时，或您以其他本公司允许的方式实际使用苏格优品商城时，您为在中国大陆地区合法设立并开展经营活动或其他业务的法人或其他组织，且您订立并履行本协议不受您所属、所居住或开展经营活动或其他业务的国家或地区法律法规的排斥。不具备前述条件的，您应立即终止注册或停止使用苏格优品商城。</p>
        <p>（四）您在使用苏格优品商城时，应自行判断交易方是否具有完全民事行为能力并自行决定是否使用苏格优品商城，且您应自行承担与此相关的所有风险。</p>
        <p>（五）您确认，您的所有交易，已经不可撤销地授权南阳苏格实业有限公司按照其制定的规则进行处理，同时本公司有权从获取您的相关信息（包括但不限于账户相关信息等）。本公司进行资金的操作完全来自于您的授权，本公司对因此给您造成的任何损失均不承担责任。但您确认，您使用苏格优品商城时，您仍应完全遵守本协议及本公司制定的各项规则及页面提示等。</p>
        <p>（六）商家和各级代理（省/市/县/社区）禁止套现和抬高物价。按苏格优品有关规定自然消费，如有违反者移交当地执法部门处理或处罚。</p>
        <h5>二、定义及解释</h5>
        <p>（一）苏格优品商城账户（或“该账户”）：指您按照本公司允许的方式取得的供您使用苏格优品商城的账户。</p>
        <p>（二）提现：在符合本公司规定和规则的情况下，您可以请求本公司将不超过您苏格优品商城账户内余额的资金入账您名下的有效中国大陆银行账户内，该银行账户开户名需与您的姓名或名称一致。除被冻结、止付或限制款项外， 本公司将于收到提现指令后的一到五个工作日内，将相应款项汇入该银行账户。您理解，根据您提供的银行账户所属银行不同，会有到账时间差异。此外，我们可能会对提现进行风险审查，因此可能导致提现延迟。您理解并同意您最终收到款项的服务是由您提供的银行账户对应的银行提供的，您需向该银行请求查证。</p>
        <h5>三、苏格优品商城</h5>
        <p>（一）注册相关</p>
        <p>您须注册并取得本公司提供给您的苏格优品商城账户，并且按照本公司要求提供相关信息完成激活后方可使用苏格优品商城。您需使用作为苏格优品商城登录名的本人手机号码登录苏格优品商城账户，并且您应当自行为苏格优品商城账户设置密码。您同意：</p>
        <p>1、按照本公司要求准确提供并在取得苏格优品商城账户后及时更新您正确、最新及完整的身份信息及相关资料。若本公司有合理理由怀疑您提供的身份信息或相关资料错误、不实、过时或不完整的，本公司有权暂停或终止向您提供部分或全部苏格优品商城服务。本公司对此不承担任何责任，您将承担因此产生的任何直接或间接支出。若因国家法律法规、部门规章或监管机构的要求，本公司需要您补充提供任何身份信息或相关资料时，如您不能及时提供，本公司有权暂停或终止向您提供部分或全部苏格优品商城服务。</p>
        <p>2、您应当准确提供并及时更新您提供的联系电话、联系地址、邮政编码等联系方式，以便本公司与您进行及时、有效联系。您应完全独自承担因通过这些联系方式无法与您取得联系而导致的您在使用本服务过程中遭受的任何损失等不利后果。您理解并同意，您有义务保持您提供的联系方式的有效性，如有变更需要更新的，您应按本公司的要求进行操作。</p>
        <p>3、您应及时更新您的身份信息及相关资料（包括但不限于身份证、户口本、护照等证件或其他身份证明文件、联系方式、作为苏格优品商城登录名的手机号码等）。因您未及时更新资料导致的一切后果，均应由您自行承担，该后果包括但不限于导致本服务无法提供或提供时发生任何错误、苏格优品商城账户及该账户内余额被别人盗用。</p>
        <p>4、您确认，本公司有权在出现下列情形时要求核对您的有效身份证件或其他必要文件，留存有效身份证件的彩色扫描件，且完成本公司要求的相关身份认证。您应积极配合，否则本公司有权限制或停止向您提供部分或全部苏格优品商城服务：</p>
        <p>A、您的苏格优品商城账户余额连续出现异常波动的；</p>
        <p>B、您要求变更身份信息或您身份信息已过有效期的；</p>
        <p>C、本公司认为您的操作行为或情况出现异常的；</p>
        <p>D、本公司认为您的身份信息或相关资料存在疑点或本公司在向您提供服务的过程中认为您已经提供的身份信息或相关资料存在疑点的；</p>
        <p>E、本公司认为应核对或留存您身份证件或其他必要文件的其他情形的。</p>
        <p>本条款所称“以上”,包括本数。</p>
        <p>5、您在苏格优品商城账户中设置的昵称等必须遵守法律法规、公序良俗、社会公德，且不会侵害其他第三方的合法权益，否则本公司有权对您的苏格优品商城账户采取限制措施，包括但不限于屏蔽、撤销您苏格优品商城账户的昵称，停止提供部分或全部苏格优品商城服务。</p>
        <p>（三）账户安全</p>
        <p>您将对使用苏格优品商城账户或密码、校验码进行的一切操作及言论负完全的责任，您同意：</p>
        <p>1、除相关产品另有规则外，本公司可以通过您的苏格优品商城登录名和密码或扫描二维码或者本公司认可的其他方式识别您的身份，您应当对该等苏格优品商城登录名、密码、校验码、身份识别信息等进行妥善保管，对于因苏格优品商城登录名、密码、校验码、身份识别信息等泄露所致的损失由您自行承担。您保证不向其他任何人泄露您的苏格优品商城登录名、密码、校验码以及身份信息等，亦不使用其他任何人的苏格优品商城登录名、密码、校验码、身份识别信息等。本公司亦可能通过本服务应用您使用的其他产品或设备识别您的指示，您应当妥善保管处于您或应当处于您掌控下的这些产品或设备， 对于这些产品或设备因非您本人使用或遗失所致的任何损失，由您自行承担。</p>
        <p>2、基于计算机端、手机端以及使用其他电子设备的用户使用习惯，我们可能在您使用具体苏格优品商城时设置不同的账户登录模式及采取不同的措施识别您的身份。</p>
        <p>3、您同意，（a）如您发现有他人冒用或盗用您的苏格优品商城登录名及密码或任何其他未经合法授权之情形，或发生与您的苏格优品商城账户关联的手机或其他设备遗失或其他可能危及到您的苏格优品商城账户内财产权益安全情形时，您应立即以有效方式通知本公司，向本公司申请暂停相关苏格优品商城服务。您不可撤销地授权本公司，以保障您的合法权益；及（b）确保您在持续登录苏格优品商城时段结束时，以正确步骤离开苏格优品商城。本公司不能也不会对因您未能遵守本款约定而发生的任何损失、损毁及其他不利后果负责。您理解本公司对您的请求采取行动需要合理期限，在此之前，本公司对已执行的指令及（或）所导致的您的损失不承担任何责任。</p>
        <p>4、您确认，您应自行对您的苏格优品商城账户负责，只有您本人方可使用该账户。该账户不可转让、不可赠与、不可继承，但账户内的相关财产权益可被依法继承。</p>
        <p>5、您同意，基于运行和交易安全的需要，本公司可以暂时停止提供或者限制苏格优品商城部分功能，或提供新的功能，在任何功能减少、增加或者变化时，只要您仍然使用本服务，表示您仍然同意本协议或者变更后的协议。</p>
        <p>6、本公司有权了解您使用苏格优品商城的真实背景及目的，您应如实提供本公司所需的真实、全面、准确的信息或资料；如果本公司有合理理由怀疑您提供虚假交易信息的，本公司有权暂时或永久限制您所使用苏格优品商城的部分或全部功能。</p>
        <p>7、您同意，本公司有权按照国家法律或行政法规所赋予权力的有权机关的要求提供您的个人信息或对您的支付宝账户进行查询、冻结或扣划。</p>
        <p>（四）、积分相关</p>
        <p>1、积分并非您拥有所有权的财产，本公司有权单方面调整积分数值或调整本公司的积分规则，而无须征得您的同意。</p>
        <p>2、如本公司怀疑您的积分的获得及（或）使用存有欺诈或其它本公司认为不当的行为，本公司可随时取消、限制或终止您的积分或积分使用。</p>
        <p>（五）、提现相关</p>
        <p>在您使用苏格优品商城，下面任一情况下均会导致提现不成功，您需要自行承担该风险：</p>
        <p>您的指定账户余额不足；</p>
        <p>2、您的指定账户被有权机关采取强制措施，或者已依据其他协议被冻结、止付。</p>
        <h5>四、苏格优品商城使用限制</h5>
        <p>（一）您在使用苏格优品商城时应遵守中华人民共和国相关法律法规及您所属、所居住或开展经营活动或其他业务的国家或地区的法律法规，不得将本服务用于任何非法目的，也不得以任何非法方式使用本服务。</p>
        <p>（二）您不得利用苏格优品商城从事侵害他人合法权益之行为， 否则本公司有权拒绝提供本服务，且您应承担所有相关法律责任，因此导致本公司或本公司雇员或其他方受损的，您应承担赔偿责任。</p>
        <h5>五、隐私权保护</h5>
        <p>本公司重视对用户隐私的保护。</p>
        <h5>六、系统中断或故障</h5>
        <p>本公司系统因下列状况无法正常运作，使您无法使用或无法正常使用各项服务时，本公司不承担损害赔偿责任，该状况包括但不限于：</p>
        <p>（一）本公司公告之系统停机维护期间。</p>
        <p>（二）电信设备出现故障不能正常进行数据传输的。</p>
        <p>（三）因台风、地震、海啸、洪水、停电、战争、恐怖袭击等不可抗力之因素，造成本公司系统障碍不能正常执行业务的。</p>
        <p>（四）由于黑客攻击、电信部门技术调整或故障、网站升级、银行方面的问题等原因而造成的服务中断或者延迟。</p>
        <h5>七、知识产权的保护</h5>
        <p>（一）苏格优品商城上的所有内容，包括但不限于著作、图片、档案、资讯、资料、网站架构、网站画面的安排、网页设计，均由本公司或本公司关联公司依法拥有其知识产权，包括但不限于商标权、专利权、著作权、商业秘密等。</p>
        <p>（二）非经本公司书面同意，任何人不得擅自使用、修改、反向编译、复制、公开传播、改变、散布、发行或公开发表苏格优品商城的程序或内容。</p>
        <p>（三）尊重知识产权是您应尽的义务，如有违反， 您应承担损害赔偿责任。</p>
        <h5>八、法律适用与管辖</h5>
        <p>本协议之效力、解释、变更、执行与争议解决均适用中华人民共和国法律。因本协议产生之争议，均应依照中华人民共和国法律予以处理，并由被告住所地人民法院管辖。</p>
        <button @click="readAgreement(1)">我知道了</button>
      </div>
    </div>
  </div>
</template>

<script>
  import api from '@/assets/js/api.js'
  import { imgUrl } from '@/assets/js/api.js'
  import EXIF from '@/assets/js/exif.js';
  import { Toast } from 'mint-ui'
  let token = localStorage.getItem('token')
  export default {
    name: "borrowing",
    data () {
      return {
        borrowInfo: {
          money: '1',
          imgs: '',
          isUp: false,
          realName: '',
          idNum: '',
          tel: '',
          agree: false,
          agreeNum: '0',
          upImgSrc: ''
        },
        bankAccount:'',
        bankId: '',
        aClass: '',
        an:'top:0rem;transition-duration: .8s;',
        an0:'top:100%;transition-duration: .8s;',
        btnTxt: '申请借款',
        isC: true,

        num:0,  //上传图片数量
        base64:'' ,  //压缩后的图片
        imgData:[],   //图片
        data:{base64:''},
        imgType:2,     //图片上传的状态  0：图片已经成功上传  1表示图片在上传中 2表示图片还没上传,
        img_loading:false,
        Orientation:''  ,    //图片的拍摄角度

      }
    },
    methods: {
      // 上传图片
      addImg (event) {
        let that=this;
        let inputDOM = this.$refs.inputer;
        // console.log(inputDOM)
        // 通过DOM取文件数据
        for(let i in inputDOM.files){
          this.file= inputDOM.files[i];
          this.imgPreview(this.file);
          EXIF.getData(this.file, function() {
            that.Orientation = EXIF.getTag(this, 'Orientation');
          });
        }
      },
      imgPreview (file) {   //base64 格式
        this.imgType = 1;
        this.img_loading = true;
        let self = this;
        let imgContent = {};
        imgContent.name = file.name;
        // 看支持不支持FileReader
        if (!file || !window.FileReader) return;

        if (/^image/.test(file.type)) {
          // 创建一个reader
          var reader = new FileReader();
          // 将图片将转成 base64 格式
          reader.readAsDataURL(file);
          // 读取成功后的回调
          reader.onloadend = function () {
            let IMG = new Image();
            IMG.src = this.result;
            self.borrowInfo.imgs = IMG.src
            self.borrowInfo.isUp = true
            IMG.onload = function () {
              let w = this.naturalWidth,
                h = this.naturalHeight,
                resizeW = 0,
                resizeH = 0;
              //压缩设置
              let maxSize = {
                width: 1280,      //图片最大宽度
                height: 1280,     //图片最大高度
                level: 0.5      //图片保存质量
              };
              //计算缩放比例
              if (w > maxSize.width || h > maxSize.height) {
                let multiple = Math.max(w / maxSize.width, h / maxSize.height);
                resizeW = w / multiple;
                resizeH = h / multiple;
              } else {
                resizeW = w;
                resizeH = h;
              }
              let canvas = document.createElement("canvas"),
                cxt = canvas.getContext('2d');
              //根据拍摄的角度进行图片旋转调整
              if (self.Orientation == 3) {
                canvas.width = resizeW;
                canvas.height = resizeH;
                cxt.rotate(Math.PI);
                cxt.drawImage(IMG, 0, 0, -resizeW, -resizeH)
              } else if (self.Orientation == 8) {
                canvas.width = resizeH;
                canvas.height = resizeW;
                cxt.rotate(Math.PI * 3 / 2);
                cxt.drawImage(IMG, 0, 0, -resizeW, resizeH)
              } else if (self.Orientation == 6) {
                canvas.width = resizeH;
                canvas.height = resizeW;
                cxt.rotate(Math.PI / 2);
                cxt.drawImage(IMG, 0, 0, resizeW, -resizeH)
              } else {
                canvas.width = resizeW;
                canvas.height = resizeH;
                cxt.drawImage(IMG, 0, 0, resizeW, resizeH)
              }
              //base64,最终输出的压缩文件
              self.base64 = canvas.toDataURL('image/jpeg', maxSize.level);
              self.borrowInfo.upImgSrc = self.base64
            }
          };
        }
      },
      borrowMoney () {
        let t = this;
        if (!t.borrowInfo.money) {
          Toast('请选择贷款金额！！！');
          return false;
        };
        if (!t.borrowInfo.realName) {
          Toast('请填写真实姓名！！！');
          return false;
        };
        if (!t.borrowInfo.idNum) {
          Toast('请填写身份证账号！！！');
          return false;
        };
        if (!t.bankAccount) {
          Toast('请选择银行卡号！！！');
          return false;
        };
        if (!t.borrowInfo.tel) {
          Toast('请填写预留手机号！！！');
          return false;
        };
        if (!t.borrowInfo.isUp) {
          Toast('请上传手持身份证半身照！！！');
          return false;
        };
        if (!t.borrowInfo.agree) {
          Toast('请认真阅读并同意协议！！！');
          return false;
        };
        if (this.isC) {
          this.isC = false;
          this.btnTxt = '正在提交中，请稍等...';
          let form = this.$qs.stringify({
            token: token,
            bank_id: t.bankId,
            id_number: t.borrowInfo.idNum,
            apply_level: t.borrowInfo.money,
            name: t.borrowInfo.realName,
            phone: t.borrowInfo.tel,
            id_photo: t.borrowInfo.upImgSrc,
            agree_to_agreement: t.borrowInfo.agreeNum
          });
          api.borrowIt(form)
            .then(res => {
              if (res.code === 200) {
                Toast({
                  message: '借款成功，请等待审核，并注意您提交的账户信息变动！',
                  position: 'middle',
                  duration: 2000
                });
                setTimeout(() => {
                  this.$router.back(-1);
                },2300)
              } else {
                this.isC = true;
                this.btnTxt = '申请借款';
              }
            })
            .then(err => {
              console.log(err)
            })
        } else {
          Toast('正在提交中，请稍等...')
        }


      },
      readAgreement (idx) {
        switch (idx) {
          case 0:
            this.aClass = this.an;
            break;
          case 1:
            this.aClass = this.an0;
            break;
          default:
            return false;
        };
      },
      ag () {
        if (this.borrowInfo.agree) {
          this.borrowInfo.agreeNum = '0'
        } else {
          this.borrowInfo.agreeNum = '1'
        };
      }
    },
    created () {
      let borrow = JSON.parse(localStorage.getItem('borrow'));
      let borrowLeave = JSON.parse(localStorage.getItem('borrowLeave'));
      if (borrow) {
        this.bankAccount = borrow.card + '  ' + '（' + borrow.account + '）' ;
        this.bankId = borrow.id;
      };
      if (borrowLeave) {
        this.borrowInfo = borrowLeave;
      };
    }
  }
</script>

<style scoped lang="less">
  @import "../../assets/less/borrowing";
</style>
