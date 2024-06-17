<template>
      <div class="container">
        <div class="intro-container">
            <h1>Title</h1>
            <p>Please upload tumor images and diagnostic data to obtain MSI scores</p>
        </div>

        <div class="content-container">
            <div class="top-container">
                <el-card style="margin-right:10px;">
                    <div class="tumor-image-uploader">
                        <h2>Upload Tumor Images</h2>
                        <div style="display:flex;flex-direction:row;justify-content:center">
                            <div class="image-upload-unit">
                                <b style="margin-bottom:5px;">1/4 tumor</b>
                                <el-upload
                                    action="api/upload_CT"
                                    list-type="picture-card"
                                    :data="CT_data['1-4']"
                                    accept=".dcm"
                                    :show-file-list="false"
                                    :on-success="handleUploadCTSuccess"
                                    :on-error="handleUploadCTFail">
                                    <img v-if="CT_Url['1-4']" :src="CT_Url['1-4']" class="CTupload-avatar">
                                    <i v-else class="el-icon-plus"></i>
                                </el-upload>
                            </div>
                            <div class="image-upload-unit">
                                <b style="margin-bottom:5px;">1/2 tumor</b>
                                <el-upload
                                    action="api/upload_CT"
                                    list-type="picture-card"
                                    :data="CT_data['1-2']"
                                    accept=".dcm"
                                    :show-file-list="false"
                                    :on-success="handleUploadCTSuccess"
                                    :on-error="handleUploadCTFail">
                                    <img v-if="CT_Url['1-2']" :src="CT_Url['1-2']" class="CTupload-avatar">
                                    <i v-else class="el-icon-plus"></i>
                                </el-upload>
                            </div>
                            <div class="image-upload-unit">
                                <b style="margin-bottom:5px;">3/4 tumor</b>
                                <el-upload
                                    action="api/upload_CT"
                                    list-type="picture-card"
                                    :data="CT_data['3-4']"
                                    accept=".dcm"
                                    :show-file-list="false"
                                    :on-success="handleUploadCTSuccess"
                                    :on-error="handleUploadCTFail">
                                    <img v-if="CT_Url['3-4']" :src="CT_Url['3-4']" class="CTupload-avatar">
                                    <i v-else class="el-icon-plus"></i>
                                </el-upload>
                            </div>
                        </div>
                    </div>
                </el-card>
                <el-card style="margin-left:10px;">
                    <div class="clinical-data-uploader">
                        <h2>Upload Clinical Data</h2>
                        <div style="display: flex;flex-direction: row;">
                            <div style="display: flex;margin:0px 10px 0px 30px;">
                                <img style="width:200px;height:200px;color:black;" src="@/assets/clinical_record.svg"/>
                            </div>
                            <div style="display: flex;flex-direction: column;align-items:center;justify-content: center;flex:1 1 0;margin:0px 30px 0px 10px;">
                                <div style="margin-bottom: 10px;">
                                    <b>Completion status: &nbsp;</b>
                                    <b :style="{color:clinical_complete_num == total_clinical_num ?' #007BFF' : '#d12323'}">{{clinical_complete_num}}</b>
                                    <b>/</b>
                                    <b>{{total_clinical_num}}</b>
                                </div>
                                <el-button size="mini" style="margin-top: 10px;" type="primary" @click="clinicalFormVisible=true">
                                    <span style="font-size:18px;">Input Clinical Data</span>   
                                </el-button>
                            </div>
                        </div>
                    </div>
                </el-card>
            </div>

            <div style="margin:15px 20px;border:1.5px solid lightgray"></div>

            <div class="bottom-container">
                <div style="flex:1 1 0;position: relative;"></div>
                <el-button @click="calculate" size="mini" type="primary" style="font-size:18px;margin-right:40px;">Start Calculation</el-button>
                <div style="position:absolute;text-align:center;left:30%;width:40%;height:100%;display:flex;align-items:center;justify-content:center">
                    <div>
                        <b style="font-size:20px;">MSI score</b>
                    </div>
                    <div>
                        <b style="font-size:20px;margin:0px 10px;">:</b>
                    </div>
                    <div>
                        <b style="color:#409EFF;font-size:25px;">{{MSI_score}}</b>
                    </div>
                </div>
            </div>
        </div>

        <!-- Clinical Data Fill Form Dialog-->
        <el-dialog
            :visible.sync="clinicalFormVisible"
            :close-on-click-modal="false"
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
                    <el-upload
                        action="api/autofill_CSV"
                        :show-file-list="false"
                        :data="autofill_params"
                        accept=".csv"
                        :on-success="handleUploadCSVSuccess"
                        :on-error="handleUploadCSVFail">
                        <el-button type="text"><span style="font-size:18px">
                            Use .csv files to fill in quickly</span></el-button>
                    </el-upload>
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
                        <div slot="header"><b>Lymphocyte Count</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['lymphocyte_count']" style="width:70%;">
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
                        <div slot="header"><b>Hemoglobin</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['hemoglobin']" style="width:70%;">
                                <template slot="append">g/L</template>
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
                        <div slot="header"><b>Monocyte Count</b></div>
                        <div>
                            <el-input size="mini" v-model="clinical_data['monocyte_count']" style="width:70%;">
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

import {nanoid} from "nanoid"
import axios from "axios"
export default {
    data(){
        return {

            id:null,


            /**
             * CT
             */
            
            //CT附属的数据
            CT_data:{
                '1-4':{
                    name:'1-4',
                    nanoid:null,
                },
                '1-2':{
                    name:'1-2',
                    nanoid:null,
                },
                '3-4':{
                    name:'3-4',
                    nanoid:null,
                },
            },
            CT_Url:{
                '1-4':null,
                '1-2':null,
                '3-4':null,
            },


            /**
             * Clinical Data
             */

            clinicalFormVisible:false,
            clinical_data:{
                'gender':null,
                'age':0,
                'history_of_diabetes':null,//糖尿病史 without with
                'history_of_hypertension':null,//高血压病史 without with
                'smoking_history':null,//吸烟史 without with
                'drinking_history':null,//饮酒史 without sometimes frequently else
                'family_history_of_tumor':null,//肿瘤家族史 without with 
                'pathological_stage':null,//病理分期 I II III IV else
                'perineural_invasion':null,//神经周侵犯 without with else
                'pathological_type':null,//病理类型 well mix poor else
                'position':null,//位置 RCC LCC REC else
                'white_blood_cell_count':null,//白细胞计数
                'red_blood_cell_count':null,//红细胞计数
                'hemoglobin':null,//血色素
                'platelet_concentration':null,//血小板计数
                'neutrophil_count':null,//中性粒细胞计数
                'lymphocyte_count':null,//淋巴细胞计数 TODO
                'monocyte_count':null,//单核细胞计数 TODO
                'red_cell_volumn_distribution_width':null,//红细胞体积分布宽度
                'plateletcrit':null,//血小板比容
                'mean_platelet_volume':null,//平均血小板体积
                'albumin':null,//白蛋白
                'globulin':null,//球蛋白
                'albumin_globulin_ratio':null,//白球比值
                'blood_glucose':null,//血糖
                'triglyceride':null,//甘油三酯
                'cholesterol':null,//胆固醇
                'high_density_lipoprotein':null,//高密度脂蛋白
                'low_density_lipoprotein':null,//低密度脂蛋白
                'carcinoembryonic_antigen':null,//CEA
                'carcinoembryonic_antigen_199':null,//CA199
                'carcinoembryonic_antigen_125':null,//CA125
            },
            clinical_complete_num:23,//已经填写的临床数据数目
            total_clinical_num:32,//总的临床数据数目
            clinical_transformer:{ //字符属性的编码表
                'gender':{
                    'male':[0],
                    'female':[1],
                },
                'history_of_diabetes':{
                    'without':[0],
                    'with':[1],
                },
                'history_of_hypertension':{
                    'without':[0],
                    'with':[1],
                },
                'smoking_history':{
                    'without':[0],
                    'with':[1],
                },
                'drinking_history':{
                    'without':[1,0,0,0],
                    'sometimes':[0, 1, 0, 0],
                    'frequently':[0, 0, 1, 0],
                    'else':[0, 0, 0, 1],
                },
                'family_history_of_tumor':{
                    'without':[0],
                    'with':[1],
                },
                'pathological_stage':{
                    'I':[1, 0, 0, 0, 0],
                    'II':[0, 1, 0, 0, 0],
                    'III':[0, 0, 1, 0, 0],
                    'IV':[0, 0, 0, 1, 0],
                    'else':[0, 0, 0, 0, 1]
                },
                'perineural_invasion':{
                    'without':[1, 0, 0],
                    'with':[0, 1, 0],
                    'else':[0, 0, 1],
                },
                'pathological_type':{
                    'well':[1, 0, 0, 0],
                    'mix':[0, 1, 0, 0],
                    'poor':[0, 0, 1, 0],
                    'else':[0, 0, 0, 1],
                },
                'position':{
                    'RCC':[1,0,0,0],
                    'LCC':[0, 1, 0, 0],
                    'REC':[0, 0, 1, 0],
                    'else':[0, 0, 0, 1],
                }

            },
            autofill_params:{
                'id':null
            },

            MSI_score:'No Score',//结果
            

        }
    },
    methods:{

        calculate(){//计算结果
            const self = this
            let clinical_data = JSON.parse(JSON.stringify(this.clinical_data))
            //数据转换
            for(let key in clinical_data){
                if(typeof clinical_data[key]=='string'){
                    clinical_data[key] = this.clinical_transformer[key][clinical_data[key]]
                }
                else{
                    clinical_data[key] = [clinical_data[key]]
                }
            }
            console.log('clinical_data',clinical_data)
            let CT_data = {
                CT_list:['1-4','1-2','3-4'],
            }
            axios({
                method:"post",
                url:"/api/calc",
                data:{
                    'id':this.id,
                    'clinical_data':clinical_data,
                    'CT_data':CT_data,
                }
            }).then((res)=>{
                console.log('res:',res)
                self.MSI_score = res.data.score.toFixed(5)
                this.$message({
                    'message':'Calculate successfully',
                    'type':'success'
                })
            }).catch((err)=>{
                console.log('err:',err)
                this.$message({
                    'message':'Calculate failed',
                    'type':'error'
                })
            })

        },

        handleUploadCTSuccess(response,file,fileList){
            console.log('res:',response)
            this.CT_Url[response.name] = response.url + '?' + new Date().getTime()
        },
        handleUploadCTFail(err,file,fileList){
            console.log('res:',err)
        },
        handleUploadCSVSuccess(response,file,fileList){
            console.log('csv_res:',response)
            for(let key in response){
                this.clinical_data[key] = response[key]
            }
            this.$message({
                'message':'Autofill successfully',
                'type':'success'
            })
            
        },
        handleUploadCSVFail(err,file,fileList){
            console.log('res:',err)
            this.$message({
                'message':'Autofill failed',
                'type':'error'
            })
        },
        instanceClose(event){

            navigator.sendBeacon("/api/clear",JSON.stringify({'id':this.id}))
        }
    },

    watch:{
        clinical_data:{
            deep:true,
            immediate:true,
            handler(newValue,oldValue){
                let tempValidCount = 0;
                for(let key in this.clinical_data){
                    if(this.clinical_data[key] === undefined || this.clinical_data[key] === null || this.clinical_data[key] == ''){
                        continue;
                    }
                    else{
                        tempValidCount = tempValidCount+1;
                    }
                }
                this.clinical_complete_num = tempValidCount
            }
        }
    },

    mounted(){
        //获取id
        this.id = nanoid();
        for(let key in this.CT_data){
            this.CT_data[key]['nanoid'] = this.id;
        }
        this.autofill_params['nanoid'] = this.id;


    },



}
</script>

<style>
        .container {
            background-color: #fdfdfd;
            padding: 2rem;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            width:1200px;
            display: flex;
            flex-direction: column;
        }

        .intro-container{
            display: flex;
            flex-direction: column;
            margin-bottom:25px;
        }

        .content-container{
            display: flex;
            flex-direction: column;
            align-items: stretch;
        }

        .top-container{
            display: flex;
            justify-content: space-around;
            margin-bottom:10px;
        }

        .bottom-container{
            display: flex;
            position: relative;
        }

        .tumor-image-uploader{
            flex:1 1 0;
        }

        .clinical-data-uploader{
            display:flex;
            flex-direction: column;
            align-items: stretch;
            flex: 1 1 0;
        }

        .CTupload-avatar{
            height: 100%;
            width: 100%;
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