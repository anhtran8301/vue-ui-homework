<template>
    <div class="text-center my-3">
        <div class="mb-2"><b>DANH SÁCH CHỦ ĐỀ</b></div>
        <v-btn elevation="0" color="success" @click="openDialogUpdateChuDe(null)">
            <v-icon class="mr-2">mdi-plus-circle</v-icon>
            Thêm mới
        </v-btn>
    </div>

    <div v-if="errorMessage != ''" class="mt-2 mb-2 text-center" :style="{ color: colorMessage }">
        <b>{{ errorMessage }}</b>
    </div>

    <v-table style="width: 400px" class="mx-auto">
        <thead>
            <tr>
                <th> Mã CD </th>
                <th class="text-left"> Tên chủ đề </th>
                <th> Sửa </th>
                <th> Xoá </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in dataChuDe" :key="item.maCD">
                <td>{{ item.MaCD }}</td>
                <td>{{ item.TenChuDe }}</td>
                <td><v-icon @click="openDialogUpdateChuDe(item)">mdi-pencil</v-icon></td>
                <td><v-icon @click="showDialogDeleteConfirm(item)">mdi-delete</v-icon></td>
            </tr>
        </tbody>
    </v-table>

    <!-- Hộp thoại Thêm, sửa dữ liệu -->
    <v-dialog width="400" scrollable v-model="showDialogUpdate">
        <template v-slot:default="{ isActive }">
            <v-card prepend-icon="mdi-earth" :title="dialogUpdateTitle">
                <v-divider class="mb-3"></v-divider>

                <v-card-text class="px-4">
                    <v-text-field v-model="objChuDe.TenChuDe" label="Tên chủ đề" variant="outlined"></v-text-field>
                </v-card-text>

                <v-divider></v-divider>

                <v-card-actions>
                    <v-btn text="ĐÓNG" @click="isActive.value = false"></v-btn>
                    <v-spacer></v-spacer>
                    <v-btn color="surface-variant" text="LƯU" variant="flat" @click="saveUpdateAction()"></v-btn>
                </v-card-actions>
            </v-card>
        </template>
    </v-dialog>

    <!-- Hộp thoại xác nhận xóa -->
    <v-dialog v-model="showDialogDelete" max-width="400px">
        <v-card>
            <v-card-title class="headline">Xác nhận xoá</v-card-title>
            <v-divider class="mb-4"></v-divider>
            <v-card-text class="text-center">
                Bạn có chắc xoá dữ liệu không?
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue" text @click="showDialogDelete = false">Huỷ</v-btn>
                <v-btn color="red" text @click="deleteAction()">Đồng ý</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>

</template>
<script>
import axios from '../utils/axios'
export default {
    name: 'ChuDeComponent',
    data: () => ({
        dataChuDe: [],
        showDialogUpdate: false,
        showDialogDelete: false,
        dialogUpdateTitle: "",
        objChuDe: {
            MaCD: 0,
            TenChuDe: ""
        },
        errorMessage: "",
        colorMessage: "blue"
    }),
    mounted() {
        this.getChuDe()
    },
    methods: {
        // Lấy danh sách chủ đề
        getChuDe() {
            axios.get('/chude', {})
                .then((response) => { this.dataChuDe = [...response.data.data] })
                .catch((error) => { console.log(error); });
        },

        // Mở hộp thoại thêm hoặc sửa thông tin chủ đề
        openDialogUpdateChuDe(obj) {
            this.showDialogUpdate = true;
            if (obj == null) {
                this.dialogUpdateTitle = "Thêm mới chủ đề"
                this.objChuDe.MaCD = 0;
                this.objChuDe.TenChuDe = "";
            }
            else {
                this.dialogUpdateTitle = "Chỉnh sửa thông tin chủ đề"
                this.objChuDe.MaCD = obj.MaCD;
                this.objChuDe.TenChuDe = obj.TenChuDe;
            }
        },

        // Thực hiện thao tác thêm hoặc chỉnh sửa thông tin
        saveUpdateAction() {
            if (this.objChuDe.TenChuDe == "")
                return;

            axios.post('/chude/update',
                {
                    MaCD: this.objChuDe.MaCD,
                    TenChuDe: this.objChuDe.TenChuDe
                })
                .then((response) => {
                    this.showDialogUpdate = false;
                    this.getChuDe();
                    this.errorMessage = response.data.message;
                    this.colorMessage = "blue";
                })
                .catch((error) => {
                    this.errorMessage = error.response.data.message;
                    this.colorMessage = "red";
                    this.showDialogUpdate = false;
                });
        },

        // Hiển thị hộp thoại Confirm trước khi xoá
        showDialogDeleteConfirm(obj) {
            console.log("truoc khi mo hop thoai",this.objChuDe)
            // Lưu giữ thông tin mã chủ đề cần xoá
            this.objChuDe.MaCD = obj.MaCD;
            this.objChuDe.TenChuDe = obj.TenChuDe;
           
            this.showDialogDelete = true;
             console.log("sau khi mo hop thoai",this.objChuDe)
        },

        // Xoá dữ liệu
        deleteAction() {
            axios.post('/chude/delete',
                {
                    MaCD: this.objChuDe.MaCD
                })
                .then((response) => {
                    this.getChuDe();
                    this.showDialogDelete = false;
                    this.errorMessage = response.data.message;
                    this.colorMessage = "blue";
                })
                .catch((error) => {
                    this.errorMessage = error.response.data.message;
                    this.colorMessage = "red";
                    this.showDialogDelete = false;
                });
        },
    }
}
</script>