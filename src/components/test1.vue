<template>
      <div class="container">
        <h1>Title</h1>
        <p>Please upload tumor images and diagnostic data to obtain MSI scores</p>
        
        <div class="upload-section">
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
        
        <div class="upload-section">
            <h2>Upload clinical data</h2>
            <el-upload
            class="upload-demo"
            drag
            size="mini"
            multiple>
            <i class="el-icon-upload"></i>
                <div class="el-upload__text">Drag files here, or <em>click to upload</em></div>
            </el-upload>
        </div>
        
        <button class="submit-button">Submit</button>
        
        <div class="result-section">
            <h2>MSI Score</h2>
            <div class="result-box" id="msi-score">The results are displayed here</div>
        </div>
    </div>
</template>

<script>
export default {

}
</script>

<style>
        .image-upload-unit{
            margin:10px;
        }

        body {
            font-family: Arial, sans-serif;
            background-color: #f8f9fa;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            background-color: #fff;
            padding: 2rem;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            text-align: center;
            max-width: 600px;
            width: 100%;
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

        .upload-section {
            margin-bottom: 2rem;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        h2 {
            font-size: 18px;
            margin-bottom: 1rem;
            color: #343a40;
        }

        .upload-buttons {
            display: flex;
            justify-content: space-around;
            margin-bottom: 1rem;
        }

        .upload-buttons input {
            padding: 0.5rem;
            border: 1px solid #ced4da;
            border-radius: 4px;
            background-color: #fff;
            cursor: pointer;
        }

        .submit-button {
            background-color: #007BFF;
            color: #fff;
            padding: 0.75rem 1.5rem;
            border: none;
            border-radius: 4px;
            font-size: 18px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        .submit-button:hover {
            background-color: #0056b3;
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