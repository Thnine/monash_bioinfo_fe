<template>
      <div class="container">
        <div class="intro-container">
            <h1>Title</h1>
            <p>Please upload tumor images and diagnostic data to obtain MSI scores</p>
        </div>

        <div class="content-container">
            <div class="left-container">
                <div class="tumor-image-uploader">
                    <h2>Upload Tumor Images</h2>
                    <div style="display:flex;flex-direction:row;justify-content:center">
                        <div class="image-upload-unit">
                            <b style="margin-bottom:5px;">1/4 tumor</b>
                            <el-upload
                                list-type="picture-card">
                                <i slot="default" class="el-icon-plus"></i>
                                <div slot="file" slot-scope="{file}">
                                <img
                                    class="el-upload-list__item-thumbnail"
                                    :src="file.url" alt=""
                                >
                                <span class="el-upload-list__item-actions">
                                    <span
                                    class="el-upload-list__item-preview"
                                    >
                                    <i class="el-icon-zoom-in"></i>
                                    </span>
                                    <span
                                    v-if="!disabled"
                                    class="el-upload-list__item-delete"
                                    >
                                    <i class="el-icon-download"></i>
                                    </span>
                                    <span
                                    v-if="!disabled"
                                    class="el-upload-list__item-delete"
                                    >
                                    <i class="el-icon-delete"></i>
                                    </span>
                                </span>
                                </div>
                            </el-upload>
                        </div>
                        <div class="image-upload-unit">
                            <b style="margin-bottom:5px;">1/2 tumor</b>
                            <el-upload
                                list-type="picture-card">
                                <i slot="default" class="el-icon-plus"></i>
                                <div slot="file" slot-scope="{file}">
                                <img
                                    class="el-upload-list__item-thumbnail"
                                    :src="file.url" alt=""
                                >
                                <span class="el-upload-list__item-actions">
                                    <span
                                    class="el-upload-list__item-preview"
                                    >
                                    <i class="el-icon-zoom-in"></i>
                                    </span>
                                    <span
                                    v-if="!disabled"
                                    class="el-upload-list__item-delete"
                                    >
                                    <i class="el-icon-download"></i>
                                    </span>
                                    <span
                                    v-if="!disabled"
                                    class="el-upload-list__item-delete"
                                    >
                                    <i class="el-icon-delete"></i>
                                    </span>
                                </span>
                                </div>
                            </el-upload>
                        </div>
                        <div class="image-upload-unit">
                            <b style="margin-bottom:5px;">3/4 tumor</b>
                            <el-upload
                                list-type="picture-card">
                                <i slot="default" class="el-icon-plus"></i>
                                <div slot="file" slot-scope="{file}">
                                <img
                                    class="el-upload-list__item-thumbnail"
                                    :src="file.url" alt=""
                                >
                                <span class="el-upload-list__item-actions">
                                    <span
                                    class="el-upload-list__item-preview"
                                    >
                                    <i class="el-icon-zoom-in"></i>
                                    </span>
                                    <span
                                    v-if="!disabled"
                                    class="el-upload-list__item-delete"
                                    >
                                    <i class="el-icon-download"></i>
                                    </span>
                                    <span
                                    v-if="!disabled"
                                    class="el-upload-list__item-delete"
                                    >
                                    <i class="el-icon-delete"></i>
                                    </span>
                                </span>
                                </div>
                            </el-upload>
                        </div>
                    </div>
                </div>
                <div class="clinical-data-uploader">
                    <h2>Upload Clinical Data</h2>
                    <el-button type="text" @click="clinicalFormVisible=true">Click to fill in diagnostic data</el-button>
                </div>
            </div>
            <div class="right-container">
                <el-button type="primary" style="font-size:18px;">Start Calculation</el-button>
        
                <div class="result-shower">
                    <h2>MSI Score</h2>
                    <div class="result-box" id="msi-score">The results are displayed here</div>
                </div>

            </div>
        </div>

        <!-- Clinical Data Fill Form Dialog-->
        <el-dialog
            title="Clinical Form"
            :visible.sync="clinicalFormVisible"
            width="50%">

            <el-form>
                <el-form-item label-width="200px" label="gender">
                    <el-radio v-model="clinical_data['gender']" :label="'male'">Male</el-radio>
                    <el-radio v-model="clinical_data['gender']" :label="'female'">Female</el-radio>
                </el-form-item>
                <el-form-item label-width="200px" label="age">
                    <el-input-number v-model="clinical_data['age']" size="mini" :min="0"></el-input-number>
                </el-form-item>
                <el-form-item label-width="200px" label="history of diabetes">
                    <el-radio v-model="clinical_data['history_of_diabetes']" :label="'without'">No</el-radio>
                    <el-radio v-model="clinical_data['history_of_diabetes']" :label="'with'">Yes</el-radio>
                </el-form-item>
                <el-form-item label-width="200px" label="history of hypertension">
                    <el-radio v-model="clinical_data['history_of_hypertension']" :label="'without'">No</el-radio>
                    <el-radio v-model="clinical_data['history_of_hypertension']" :label="'with'">Yes</el-radio>
                </el-form-item>
                <el-form-item label-width="200px" label="smoking history">
                    <el-radio v-model="clinical_data['smoking_history']" :label="'without'">No</el-radio>
                    <el-radio v-model="clinical_data['smoking_history']" :label="'with'">Yes</el-radio>
                </el-form-item>
                <el-form-item label-width="200px" label="drinking history">
                    <el-radio v-model="clinical_data['drinking_history']" :label="'without'">No</el-radio>
                    <el-radio v-model="clinical_data['drinking_history']" :label="'sometimes'">Sometimes</el-radio>
                    <el-radio v-model="clinical_data['drinking_history']" :label="'frequently'">Frequently</el-radio>
                    <el-radio v-model="clinical_data['drinking_history']" :label="'else'">Else</el-radio>
                </el-form-item>
                <el-form-item label-width="200px" label="family history of tumor">
                    <el-radio v-model="clinical_data['family_history_of_tumor']" :label="'without'">No</el-radio>
                    <el-radio v-model="clinical_data['family_history_of_tumor']" :label="'with'">Yes</el-radio>
                </el-form-item>
                <el-form-item label-width="200px" label="pathological stage">
                    <el-radio v-model="clinical_data['pathological_stage']" :label="'I'">I</el-radio>
                    <el-radio v-model="clinical_data['pathological_stage']" :label="'II'">II</el-radio>
                    <el-radio v-model="clinical_data['pathological_stage']" :label="'III'">III</el-radio>
                    <el-radio v-model="clinical_data['pathological_stage']" :label="'IV'">IV</el-radio>
                    <el-radio v-model="clinical_data['pathological_stage']" :label="'else'">else</el-radio>
                </el-form-item>
                <el-form-item label-width="200px" label="perineural invasion">
                    <el-radio v-model="clinical_data['perineural_invasion']" :label="'without'">No</el-radio>
                    <el-radio v-model="clinical_data['perineural_invasion']" :label="'with'">Yes</el-radio>
                    <el-radio v-model="clinical_data['perineural_invasion']" :label="'else'">Else</el-radio>
                </el-form-item>
                <el-form-item label-width="200px" label="pathological_type">
                    <el-radio v-model="clinical_data['pathological_type']" :label="'well'">Well</el-radio>
                    <el-radio v-model="clinical_data['pathological_type']" :label="'mix'">Mix</el-radio>
                    <el-radio v-model="clinical_data['pathological_type']" :label="'poor'">Poor</el-radio>
                    <el-radio v-model="clinical_data['pathological_type']" :label="'else'">Else</el-radio>
                </el-form-item>
                <el-form-item label-width="200px" label="position">
                    <el-radio v-model="clinical_data['position']" :label="'RCC'">RCC</el-radio>
                    <el-radio v-model="clinical_data['position']" :label="'LCC'">LCC</el-radio>
                    <el-radio v-model="clinical_data['position']" :label="'REC'">REC</el-radio>
                    <el-radio v-model="clinical_data['position']" :label="'else'">Else</el-radio>
                </el-form-item>
                <el-form-item label="white blood cell count">
                    <el-input size="mini" v-model="clinical_data['white_blood_cell_count']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="red blood cell count">
                    <el-input size="mini" v-model="clinical_data['red_blood_cell_count']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="platelet concentration">
                    <el-input size="mini" v-model="clinical_data['platelet_concentration']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="neutrophil count">
                    <el-input size="mini" v-model="clinical_data['neutrophil_count']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="lymphocyte count">
                    <el-input size="mini" label-width="200px" v-model="clinical_data['lymphocyte_count']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="red cell volumn distribution width">
                    <el-input size="mini" v-model="clinical_data['red_cell_volumn_distribution_width']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="plateletcrit">
                    <el-input size="mini" v-model="clinical_data['plateletcrit']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="mean platelet volume">
                    <el-input size="mini" v-model="clinical_data['mean_platelet_volume']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="albumin">
                    <el-input size="mini" v-model="clinical_data['albumin']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="globulin">
                    <el-input size="mini" v-model="clinical_data['globulin']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="albumin globulin ratio">
                    <el-input size="mini" v-model="clinical_data['albumin_globulin_ratio']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="blood glucose">
                    <el-input size="mini" v-model="clinical_data['blood_glucose']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="triglyceride">
                    <el-input size="mini" v-model="clinical_data['triglyceride']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="cholesterol">
                    <el-input size="mini" v-model="clinical_data['cholesterol']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="high density lipoprotein">
                    <el-input size="mini" v-model="clinical_data['high_density_lipoprotein']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="low density lipoprotein">
                    <el-input size="mini" v-model="clinical_data['low_density_lipoprotein']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="carcinoembryonic antigen">
                    <el-input size="mini" v-model="clinical_data['carcinoembryonic_antigen']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="carcinoembryonic antigen 199">
                    <el-input size="mini" v-model="clinical_data['carcinoembryonic_antigen_199']" style="width:300px;"></el-input>
                </el-form-item>
                <el-form-item label="carcinoembryonic antigen 125">
                    <el-input size="mini" v-model="clinical_data['carcinoembryonic_antigen_125']" style="width:300px;"></el-input>
                </el-form-item>
            </el-form>
            

            <span slot="footer">
                <el-button @click="clinicalFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="clinicalFormVisible = false">确 定</el-button>
            </span>
        </el-dialog>


    </div>
</template>

<script>
export default {
    data(){
        return {
            clinicalFormVisible:false,
            
            clinical_data:{
                'gender':'male',
                'age':64,
                'history_of_diabetes':'without',//糖尿病史 without with
                'history_of_hypertension':'with',//高血压病史 without with
                'smoking_history':'with',//吸烟史 without with
                'drinking_history':'else',//饮酒史 without sometimes frequently else
                'family_history_of_tumor':'without',//肿瘤家族史 without with 
                'pathological_stage':'II',//病理分期 I II III IV else
                'perineural_invasion':'else',//神经周侵犯 without with else
                'pathological_type':'well',//病理类型 well mix poor else
                'position':'RCC',//位置 RCC LCC REC else
                'white_blood_cell_count':6.1,//白细胞计数
                'red_blood_cell_count':4.19,//红细胞计数
                'platelet_concentration':153.0,//血小板计数
                'neutrophil_count':3.8,//中性粒细胞计数
                'lymphocyte_count':1.9,//淋巴细胞计数
                'red_cell_volumn_distribution_width':14.4,//红细胞体积分布宽度
                'plateletcrit':0.2,//血小板比容
                'mean_platelet_volume':11.3,//平均血小板体积
                'albumin':42.8,//白蛋白
                'globulin':20.8,//球蛋白
                'albumin_globulin_ratio':2.1,//白球比值
                'blood_glucose':5.03,//血糖
                'triglyceride':1.23,//甘油三酯
                'cholesterol':4.42,//胆固醇
                'high_density_lipoprotein':1.61,//高密度脂蛋白
                'low_density_lipoprotein':2.46,//低密度脂蛋白
                'carcinoembryonic_antigen':0.2,//CEA
                'carcinoembryonic_antigen_199':2.29,//CA199
                'carcinoembryonic_antigen_125':1.08,//CA125
            }
            

        }
    },
    methods:{
        test(){
            console.log(this.clinical_data)
        }
    }
}
</script>

<style>
        .container {
            background-color: #fff;
            padding: 2rem;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            text-align: center;
            width:1200px;
            height:600px;
            display: flex;
            flex-direction: column;
        }

        .intro-container{
            display: flex;
            flex-direction: column;
        }

        .content-container{
            display: flex;
        }

        .left-container{

        }

        .right-container{

        }

        .tumor-image-uploader{
            
        }

        .clinical-data-uploader{

        }

        .result-shower{

        }

        .image-upload-unit{
            margin:10px;
        }


        h1 {
            font-size: 24px;
            margin-bottom: 1rem;
            color: #007BFF;
        }

        p {
            font-size: 18px;
            margin-bottom: 2rem;
        }

        h2 {
            font-size: 18px;
            margin-bottom: 1rem;
            color: #343a40;
        }

        .result-section {
            margin-top: 2rem;
        }

        .result-box {
            border: 1px solid #ced4da;
            border-radius: 4px;
            padding: 1rem;
            background-color: #f8f9fa;
            font-size: 18px;
            color: #495057;
        }
</style>