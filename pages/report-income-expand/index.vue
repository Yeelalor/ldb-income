<template>
    <div class="pt-4">
        <v-dialog width="200" v-model="loading" persistent>
            <v-card height="40" class="pt-5 px-5" color="#338ABF">
                <v-progress-linear indeterminate color="white"></v-progress-linear>
            </v-card>
        </v-dialog>
        <span style="background-color:#f5f5f5;width:100px;border-radius:20px" class="px-4 py-1"><v-icon color="#999"
                class="mr-2">mdi-book-open-variant</v-icon>ລາຍງານລາຍຮັບ</span><br /><br />
        <v-row>
            <v-col cols="12" md="3" sm="4">
                <v-menu ref="start_go_menu" v-model="start_go_menu" :close-on-content-click="false"
                    :return-value.sync="start_go_date" transition="scale-transition" offset-y min-width="auto">
                    <template v-slot:activator="{ on, attrs }">
                        <v-text-field dense outlined v-model="start_go_date" required label="ວັນທີເລີ່ມຕົ້ນ"
                            prepend-inner-icon="mdi-calendar" background-color="#F5F5F5" readonly v-bind="attrs"
                            v-on="on"></v-text-field>
                    </template>
                    <v-date-picker v-model="start_go_date" no-title scrollable
                        @input="$refs.start_go_menu.save(start_go_date)">
                        <v-spacer></v-spacer>
                    </v-date-picker>
                </v-menu>
            </v-col>
            <v-col cols="12" md="3" sm="4">
                <v-menu ref="end_menu" v-model="end_menu" :close-on-content-click="false" :return-value.sync="end_date"
                    transition="scale-transition" offset-y min-width="auto">
                    <template v-slot:activator="{ on, attrs }">
                        <v-text-field dense outlined v-model="end_date" required label="ວັນທີສຸດທ້າຍ"
                            prepend-inner-icon="mdi-calendar" background-color="#F5F5F5" readonly v-bind="attrs"
                            v-on="on"></v-text-field>
                    </template>
                    <v-date-picker v-model="end_date" no-title scrollable @input="$refs.end_menu.save(end_date)">
                        <v-spacer></v-spacer>
                    </v-date-picker>
                </v-menu>
            </v-col>
            <v-col cols="12" md="3" sm="4"><v-select v-model="pl" :items="items" item-text="name" item-value="value"
                    background-color="#F5F5F5" outlined dense label="ເລືອກ GL"></v-select></v-col>
            <v-col cols="12" md="3" sm="4">

            </v-col>
        </v-row>
        <v-btn color="green" elevation="0" rounded class="white--text" @click="onSchIncome">
            <v-icon>mdi-search-web</v-icon>
            ຄົ້ນຫາ
        </v-btn>
        <br /><br />

        <v-divider></v-divider>
        <v-data-table :items="report_data_list" dense :headers="report_header" no-data-text="ຍັງບໍ່ມີລາຍການ">
            <template v-slot:item="row">
                <tr :style="{ 'background-color': row?.index % 2 === 0 ? '#F5F5F5' : '#fff' }">
                    <td>{{ row?.item?.code }}</td>
                    <td>{{ row?.item?.gl }}</td>
                    <td>{{ row?.item?.gl1 }}</td>
                    <td>{{ (row?.item?.debit).toString()
                        .replace(/\B(?=(\d{3})+(?!\d))/g, ',') }}</td>
                    <td>{{ (row?.item?.credit).toString()
                        .replace(/\B(?=(\d{3})+(?!\d))/g, ',') }}</td>
                    <td>{{ (row?.item?.balance).toString()
                        .replace(/\B(?=(\d{3})+(?!\d))/g, ',') }}</td>
                </tr>
            </template>
        </v-data-table>
        <!-- <vue-excel-xlsx :data="data" :fields="columns" worksheet="My Worksheet" name="filename.xls">

        </vue-excel-xlsx> -->
        <vue-excel-xlsx :data="data" :columns="columns" :file-name="'filename'" :file-type="'xlsx'"
            :sheet-name="'sheetname'" v-if="report_data_list?.length > 0">
            <v-btn color="green" rounded elevation="0" class="white--text" @click="onSchIncome">
                <v-icon>mdi-cloud-download-outline</v-icon>
                <span class="ml-2">ດາວໂຫຼດ excel</span>
            </v-btn>
        </vue-excel-xlsx>

    </div>
</template>

<script>
export default {
    data() {
        return {
            loading: false,
            start_go_menu: false,
            end_menu: false,
            start_go_date: null,
            end_date: null,
            pl: '',
            report_data_list: [],
            report_header: [
                { text: 'CODE', value: 'code' },
                { text: 'GL', value: 'gl' },
                { text: 'GL1', value: 'gl1' },
                { text: 'DEBIT', value: 'debit' },
                { text: 'CREDIT', value: 'credit' },
                { text: 'BALANCE', value: 'balance' },
            ],
            items: [
                {
                    name: 'ລາຍຮັບ', value: '4'
                },
                {
                    name: 'ລາຍຈ່າຍ', value: '5'
                }
            ],
            columns: [
                {
                    label: 'CODE',
                    field: 'code',
                },
                {
                    label: 'GL',
                    field: 'gl',
                    dataFormat: this.priceFormat,
                },
                {
                    label: 'GL1',
                    field: 'gl1',
                },
                {
                    label: 'DEBIT',
                    field: 'debit',
                },
                {
                    label: 'CREDIT',
                    field: 'credit',
                },
                {
                    label: 'BALANCE',
                    field: 'balance',
                },
            ],
            data: [
                {
                    code: '',
                    gl: '',
                    gl1: '',
                    debit: '',
                    credit: '',
                    balance: ''
                }
            ],
        }
    },
    mounted() {
        if (!localStorage.getItem('ee4a2604-bd95-4590-ab83-c4c37cf385f8')) {
            this.$router.push('/')
        }
    },
    methods: {

        onSchIncome() {
            try {
                let data =
                {
                    pl: this.pl,
                    startDate: this.start_go_date,
                    endDate: this.end_date
                }
                this.loading = true
                console.log("itemssend:", data)
                this.$axios.$post('/getIncome', data).then((data) => {
                    console.log("schData:===", data)
                    this.report_data_list = data;
                    this.data = data?.map((list) => {
                        return {
                            code: list?.code,
                            gl: list?.gl,
                            gl1: list?.gl1,
                            debit: list?.debit,
                            credit: list?.credit,
                            balance: list?.balance
                        };
                    })
                    this.loading = false
                })
            } catch (error) {
                this.loading = false
                console.log(error)
            }
        }
    }
}
</script>

<style></style>