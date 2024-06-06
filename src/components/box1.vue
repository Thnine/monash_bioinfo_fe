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
            :visible.sync="clinicalFormVisible"
            width="40%">

            <div class="clinical-form-intro-container">
                <div style="display:flex;flex-direction:row;align-items:center">
                    <b style="font-size:31px">Clinical Data Collection Form</b>

                    <div style="flex:1 1 0"></div>
                </div>
                <div>
                    <p style="font-size:18px;text-align:left;word-break:keep-all;">
                        Please fill in the following clinical data carefully and cautiously, as a prerequisite for the implementation of our algorithm.
                    </p>
                </div>
                <div style="text-align:left">
                    <el-button type="text"><span style="font-size:18px">Use .csv files to fill in quickly</span></el-button>
                </div>

            </div>

            <div class="clinical-form-content-container">

                <div style="display:flex;flex-direction:column;flex:1 1 0;padding-right:15px;">
                    <!--content left column-->
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Gender</b></div>
                        <div>
                            <el-radio v-model="clinical_data['gender']" :label="'male'">Male</el-radio>
                            <el-radio v-model="clinical_data['gender']" :label="'female'">Female</el-radio>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>History of Diabetes</b></div>
                        <div>
                            <el-radio v-model="clinical_data['history_of_diabetes']" :label="'without'">No</el-radio>
                            <el-radio v-model="clinical_data['history_of_diabetes']" :label="'with'">Yes</el-radio>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Smoking History</b></div>
                        <div>
                            <el-radio v-model="clinical_data['smoking_history']" :label="'without'">No</el-radio>
                            <el-radio v-model="clinical_data['smoking_history']" :label="'with'">Yes</el-radio>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Family History Of Tumor</b></div>
                        <div>
                            <el-radio v-model="clinical_data['family_history_of_tumor']" :label="'without'">No</el-radio>
                            <el-radio v-model="clinical_data['family_history_of_tumor']" :label="'with'">Yes</el-radio>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Perineural Invasion</b></div>
                        <div>
                            <el-radio-group v-model="clinical_data['perineural_invasion']" style="width:100%;text-align:left">
                                <el-row style="margin-bottom:10px;text-align:center">
                                    <el-col :span="8">
                                        <el-radio :label="'without'">No</el-radio>
                                    </el-col>
                                    <el-col :span="8">
                                        <el-radio :label="'with'">Yes</el-radio>
                                    </el-col>
                                    <el-col :span="8">
                                        <el-radio :label="'else'">Else</el-radio>
                                    </el-col>
                                </el-row>
                            </el-radio-group>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Position</b></div>
                        <div>
                            <el-radio-group v-model="clinical_data['position']" style="width:100%;text-align:left">
                                <el-row style="margin-bottom:10px;">
                                    <el-col :span="12">
                                        <el-radio :label="'RCC'">RCC</el-radio>
                                    </el-col>
                                    <el-col :span="12">
                                        <el-radio :label="'LCC'">LCC</el-radio>
                                    </el-col>
                                </el-row>
                                <el-row>
                                    <el-col :span="12">
                                        <el-radio :label="'REC'">REC</el-radio>
                                    </el-col>
                                    <el-col :span="12">
                                        <el-radio :label="'else'">Else</el-radio>
                                    </el-col>
                                </el-row>
                            </el-radio-group>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Red Blood Cell Count</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['red_blood_cell_count']" style="width:70%;">
                                <template slot="append">*109/L</template>
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Neutrophil Count</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['neutrophil_count']" style="width:70%;">
                                <template slot="append">*109/L</template>
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Plateletcrit</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['plateletcrit']" style="width:70%;">
                                <template slot="append">PCT</template>
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Albumin</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['albumin']" style="width:70%;">
                                <template slot="append">g/L</template>
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Albumin Globulin Ratio</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['albumin_globulin_ratio']" style="width:70%;">
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Triglyceride</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['triglyceride']" style="width:70%;">
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>High Density Lipoprotein</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['high_density_lipoprotein']" style="width:70%;">
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Carcinoembryonic Antigen</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['carcinoembryonic_antigen']" style="width:70%;">
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Carcinoembryonic Antigen 125</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['carcinoembryonic_antigen_125']" style="width:70%;">
                            </el-input>
                        </div>
                    </el-card>
                </div>




                <div style="display:flex;flex-direction:column;flex:1 1 0;padding-left:15px;">
                    <!--content right column-->
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Age</b></div>
                        <div>
                            <el-input-number v-model="clinical_data['age']" size="mini" :min="0" style="width:70%"></el-input-number>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>History of Hypertension</b></div>
                        <div>
                            <el-radio v-model="clinical_data['history_of_hypertension']" :label="'without'">No</el-radio>
                            <el-radio v-model="clinical_data['history_of_hypertension']" :label="'with'">Yes</el-radio>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Drinking History</b></div>
                        <div>
                            <el-radio-group v-model="clinical_data['drinking_history']" style="width:100%;text-align:left">
                                <el-row style="margin-bottom:10px;">
                                    <el-col :span="12">
                                        <el-radio :label="'without'">No</el-radio>
                                    </el-col>
                                    <el-col :span="12">
                                        <el-radio :label="'sometimes'">Sometimes</el-radio>
                                    </el-col>
                                </el-row>
                                <el-row>
                                    <el-col :span="12">
                                        <el-radio :label="'frequently'">Frequently</el-radio>
                                    </el-col>
                                    <el-col :span="12">
                                        <el-radio :label="'else'">Else</el-radio>
                                    </el-col>
                                </el-row>
                            </el-radio-group>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Pathological Stage</b></div>
                        <div>
                            <el-radio-group v-model="clinical_data['pathological_stage']" style="width:100%;text-align:left">
                                <el-row style="margin-bottom:10px;">
                                    <el-col :span="12">
                                        <el-radio :label="'I'">I</el-radio>
                                    </el-col>
                                    <el-col :span="12">
                                        <el-radio :label="'II'">II</el-radio>
                                    </el-col>
                                </el-row>
                                <el-row style="margin-bottom:10px;">
                                    <el-col :span="12">
                                        <el-radio :label="'III'">III</el-radio>
                                    </el-col>
                                    <el-col :span="12">
                                        <el-radio :label="'IV'">IV</el-radio>
                                    </el-col>
                                </el-row>
                                <el-row>
                                    <el-col :span="12">
                                        <el-radio :label="'else'">Else</el-radio>
                                    </el-col>
                                    <el-col :span="12">
                                    </el-col>
                                </el-row>
                            </el-radio-group>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Pathological Type</b></div>
                        <div>
                            <el-radio-group v-model="clinical_data['pathological_type']" style="width:100%;text-align:left">
                                <el-row style="margin-bottom:10px;">
                                    <el-col :span="12">
                                        <el-radio :label="'well'">Well</el-radio>
                                    </el-col>
                                    <el-col :span="12">
                                        <el-radio :label="'mix'">Mix</el-radio>
                                    </el-col>
                                </el-row>
                                <el-row>
                                    <el-col :span="12">
                                        <el-radio :label="'poor'">Poor</el-radio>
                                    </el-col>
                                    <el-col :span="12">
                                        <el-radio :label="'else'">Else</el-radio>
                                    </el-col>
                                </el-row>
                            </el-radio-group>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>White Blood Cell Count</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['white_blood_cell_count']" style="width:70%;">
                                <template slot="append">*109/L</template>
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Platelet Concentration</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['platelet_concentration']" style="width:70%;">
                                <template slot="append">*109/L</template>
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Red Cell Volumn Distribution Width</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['red_cell_volumn_distribution_width']" style="width:70%;">
                                <template slot="append">RDW</template>
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Mean Platelet Volume</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['mean_platelet_volume']" style="width:70%;">
                                <template slot="append">MPV</template>
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Globulin</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['globulin']" style="width:70%;">
                                <template slot="append">g/L</template>
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Blood Glucose</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['blood_glucose']" style="width:70%;">
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Cholesterol</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['cholesterol']" style="width:70%;">
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Low Density Lipoprotein</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['low_density_lipoprotein']" style="width:70%;">
                            </el-input>
                        </div>
                    </el-card>
                    <el-card class="clinical-form-unit">
                        <div slot="header"><b>Carcinoembryonic Antigen 199</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['carcinoembryonic_antigen_199']" style="width:70%;">
                            </el-input>
                        </div>
                    </el-card>
                </div>
            </div>

            <el-button type="primary" @click="clinicalFormVisible=false" style="width:150px;">Finish</el-button>


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

        .clinical-form-intro-container{
            display: flex;
            flex-direction: column;
            align-items: stretch;
            margin:0 40px 40px 40px;
            border-bottom:2px solid lightgray;

        }

        .clinical-form-content-container{
            margin:0 40px 40px 40px;
            display: flex;
            flex-direction: row;
        }

        .clinical-form-unit{
            margin-bottom:25px;
        }


        h1 {
            font-size: 24px;
            margin-bottom: 1rem;
            color: #007BFF;
        }


        h2 {
            font-size: 18px;
            margin-bottom: 1rem;
            color: #343a40;
        }

        .el-form-item__label{
            font-size:17px;
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