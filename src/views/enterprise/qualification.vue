<style lang="less">
@import './qualification.less';
</style>

<template>
    <div>
        <div class="row-wrapper">
            <Row :gutter="10" style="width:1200px;margin:0 auto;overflow:hidden;">
                <div class="container clearfix">
                    <div class="clearfixs mr_created">
                        <!-- <qualification class="mr_myresume_l"></qualification> -->
                        <div class="mr_myresume_l">
                            <div class="base_message_content clearfix">
                                <div class="base_message_content_left fl">
                                    <div class="aptitude">单位资质认证&nbsp;&nbsp;
                                        <span class="ico icon-notice" id="auditTips"></span>
                                    </div>
                                    <div class="member member-styone" style="display: none;">
                                        <em class="bot"></em>
                                        <em class="top"></em>
                                        <p class="closedmem">×</p>
                                        <p>根据《中华人民共和国网络安全法》，使用智联平台进行互联网招聘的机构或组织，须提交机构与经办人个人的认证材料。经审核与备案通过后，方可正常使用智联平台实施招聘或用工信息发布等行为。 根据机构或组织性质不同，须提交不同的资质认证材料。详见本页“上传证照说明”。
                                        </p>
                                    </div>

                                    <p class="secondp clearfix">
                                        <span>证照名称</span>
                                        <!-- <em class="licenses-name">北京华品博睿网络技术有限公司</em> -->
                                        <label class="licenses-name" v-model="companyName" style="margin-bottom: 10px;">{{this.companyName}}</label>
                                    </p>
                                    <p class="licenses-tip">请确认单位名称与证照名称一致，如不一致将无法通过单位认证！如单位名称不正确，可在下方进行修改。</p>
                                    <div class="change-cname">
                                        <Input type="text" id="changenName" v-model="newCompanyName"></Input>
                                        <p class="t-error" ref="tError">单位名称不能为空</p>
                                        <Button type="info" @click="modifyName()" style="margin-top:10px;">修改单位名称</Button>
                                    </div>
                                    <div class="sixp mt15">
                                        <div class="uptips">
                                            <input type="hidden" value="" post="g" id="businesslicenceurl" name="businesslicenceurl">
                                            <span>
                                                <font class="asterisk fs18">*</font> 上传证照原件照片</span>
                                        </div>
                                        <p>
                                            <font class="reupload_tiptext">仅限最多上传5份资料，文件要求以JPEG,PNG 或JPG格式，大小限制在3M以内</font>
                                        </p>
                                        <Upload class="upload-btn" action="/picture/manageFile" :show-upload-list="false" :on-success="uploadSuccess" :format="['jpg','jpeg','png']">
                                            <Button type="ghost" icon="ios-cloud-upload-outline">上传照片</Button>
                                        </Upload>
                                    </div>
                                    <!-- 展示图片的区域 -->
                                    <div class="displayPhoto" ref="displayPhoto">
                                        <img :src="imageUrl" >
                                    </div>

                                    <div class="perfect_upload">
                                        <div class="perfect_uploadbox clearfix">
                                            <div class="perfect_uploadleft">
                                                <div class="loadinglicense imgmid" style="display:none;">
                                                    <img class="imginner" src="//img03.zhaopin.cn/SZProj/newrd/img/icon/loading.gif">
                                                </div>
                                            </div>
                                        </div>
                                        <div class="licence-img-group"></div>
                                        <div class="licence-condition">确保单位名称与提交审核公司一致；如为复印件、黑白扫描件，需加盖单位公章；<br> 不可使用屏幕截图或翻拍图片提交审核；上传图片不可有与智联招聘无关的标注或水印；
                                            <br> 不可使用电子版营业执照提交审核；所上传执照照片，信息需清晰完整。
                                        </div>
                                        <div class="colef7 mt15 limit-tip-one">企业营业执照仅用于审核,不会向第三方透露,请放心上传！</div>
                                    </div>

                                    <!-- 法人认证 -->
                                    <div class="aptitude">法人认证</div>
                                    <div class="secondp">
                                        <span>
                                            <font class="asterisk fs18">*</font> 法人姓名</span>
                                        <input v-model="peopleName" type="text" id="legalperson" placeholder="请输入法人姓名" post="g" require="true" msgid="legalName" msg="法人姓名不能为空|最多可输入50个字符|请输入正确的法人姓名" datatype="require|limit|enandcn" min="1" max="50" value="" name="legalperson" class="licenses-area-inplong">
                                        <font id="legalName" class="t-error"></font>
                                    </div>
                                    <div class="submit-box">
                                        <!-- <a class="btnblue190 mt20" href="javascript:void(0);" post="g" action="/company/completedata.do" id="subBtn">提交审核</a> -->
                                        <Button type="info" @click="submit()">提交审核</Button>
                                    </div>
                                </div>
                            </div>

                        </div>

                        <main-navbar :message="pageName"></main-navbar>
                    </div>
                </div>
            </Row>
        </div>
        <Row>
            <footerDiv></footerDiv>
        </Row>
    </div>
</template>

<script>
import mainNavbar from './components/main-navbar.vue'
// import qualification from './components/qualification.vue'
import footerDiv from '@/views/main-components/footer/footer.vue'
export default {
    name: 'enterprise_qualification',
    components: {
        mainNavbar,
        // qualification,
        footerDiv
    },
    data () {
        return {
            pageName: 'qualification',
            // companyProp: '',
            enterpriseId: '',
            companyName: '',
            newCompanyName: '',
            // nameInput: '',
            peopleName: '',
            imageUrl: '',
            getUrl: '/enterprise',
            submitUrl: '/enterprise',
            rows: {}
        };
    },
    methods: {
        init () {
            this.getUrl = '/enterprise';
            this.getUrl += '/' + this.enterpriseId;
            this.enterpriseId = localStorage.getItem('userid');

            this.$axios.get(this.getUrl)
                .then(response => {
                    this.companyName = response.data.name;
                })
        },
        submit () {
            this.submitUrl = '/enterprise'
            this.submitUrl += '/' + this.enterpriseId;

            this.rows = {};

            this.rows.name = this.companyName;
            this.rows.linkman = this.peopleName;
            this.rows.state = 0;
            this.rows.imageLicense = this.imageUrl;

            this.$axios.put(this.submitUrl, this.rows)
                .then(response => {
                    this.$Message.info('提交成功！');
                })
                .catch(function (error) {
                    console.log(error)

                })
        },
        modifyName () {
            if(this.newCompanyName !== ''){
                this.companyName = this.newCompanyName;
            }else{
                this.$refs.tError.style.display = "block";
            }
            
        },
        uploadSuccess(res) {
            console.log(res);
            //将上传的照片url返回来
            this.imageUrl = res;
            this.$refs.displayPhoto.style.display = "block";
        }
    },
    computed: {

    },
    mounted () {
        this.init();
    }
};
</script>
