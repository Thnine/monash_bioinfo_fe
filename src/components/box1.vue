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
                    <el-upload
                    class="upload-demo"
                    drag
                    size="mini"
                    multiple>
                    <i class="el-icon-upload"></i>
                        <div class="el-upload__text">Drag files here, or <em>click to upload</em></div>
                    </el-upload>
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









  
    </div>
</template>

<script>
export default {

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
            height:800px;
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