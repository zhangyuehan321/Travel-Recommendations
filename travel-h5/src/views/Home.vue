<template>
    <div class="page-container">
        <div class="page-header">
            <van-nav-bar title="智能旅游助手" />
        </div>
        <div class="page-content">
            <van-notice-bar
                left-icon="info-o"
                text="基于AI的智能景点介绍与行程规划系统"
                style="margin-bottom: 16px;"
            />
            <div class="card search-card">
                <div class="section-title">
                    规划你的旅程
                </div>
                <van-field
                    v-model="formData.city"
                    is-link
                    readonly
                    @click="showCityPicker = true"
                    label="目的地"
                    placeholder="请选择或输入城市"
                    style="background-color: #f7f8fa;border-radius: 8px; margin-bottom: 12px;"
                />
                <van-field
                    v-model="formData.budget"
                    label="预算(元)"
                    placeholder="请输入预算金额"
                    type="number"
                    :border="false"
                    style="background-color: #f7f8fa;border-radius: 8px; margin-bottom: 12px;"
                />
                <van-field
                    v-model="formData.days"
                    type="digit"
                    label="天数"
                    placeholder="请输入旅行天数"
                    :border="false"
                    style="background-color: #f7f8fa;border-radius: 8px; margin-bottom: 12px;"
                />
                <van-button
                    type="primary"
                    size="large"
                    round
                    block
                    :loading="isLoading"
                    @click="handleSubmit"
                >
                    开始规划
                </van-button>
            </div>
            <div class="card">
                <div class="section-title">
                    快捷入口
                </div>
                <van-grid
                    :column-num="2"
                    :gutter="12"
                >
                    <van-grid-item @click="goPage('/chat')" icon="chat-o" text="AI对话" />
                    <van-grid-item @click="goPage('/profile')" icon="user-o" text="我的" />
                </van-grid>
            </div>
            <div class="card">
                <div class="section-title">
                    热门目的地
                </div>
                <van-grid
                    :column-num="4"
                    :gutter="8"
                >
                    <van-grid-item v-for="city in popularCities" @click="selectedCity(city)" :key="city" >
                        <div class="city-tag" :class="{'active': formData.city === city}" >
                            {{ city }}
                        </div>
                    </van-grid-item>
                </van-grid>
            </div>
        </div>
        <van-popup
            v-model:show="showCityPicker"
            position="bottom"
            round
        >
            <van-picker
                v-model="pickerValues"
                title="请选择目的地"
                :columns="cityColumns"
                confirm-button-text="确认"
                cancel-button-text="取消"
                @confirm="handleCityConfirm"
                @cancel="showCityPicker = false"
            />
        </van-popup>
    </div>

</template>
<script setup>
    import {reactive, ref} from 'vue';
    import {useRouter} from 'vue-router';
    const router = useRouter();

    const formData = reactive({
        city: '',
        budget: null,
        days: null,
    });
    const showCityPicker = ref(false);
    const pickerValues = ref([]);
    const allCities = [
        '北京', '上海', '广州', '深圳', '成都', '杭州', '西安', '重庆',
        '南京', '武汉', '苏州', '长沙', '天津', '郑州', '济南', '青岛',
        '大连', '沈阳', '哈尔滨', '长春', '福州', '厦门', '南昌', '合肥',
        '昆明', '贵阳', '南宁', '桂林', '海口', '三亚', '丽江', '大理',
        '西安', '兰州', '乌鲁木齐', '拉萨', '呼和浩特', '太原', '石家庄'
    ]
    const popularCities = ['北京', '上海', '广州', '深圳', '成都', '杭州', '西安', '重庆'];

    const cityColumns = allCities.map(city => ({
        text: city,
        value: city,
    }));

    const handleCityConfirm = ({ selectedValues }) => {
        formData.city = selectedValues[0] ?? '';
        showCityPicker.value = false;
    }

    const isLoading = ref(false);

    const handleSubmit = async () => {
        // if (isLoading.value) return;
        isLoading.value = true;
        // await new Promise(resolve => setTimeout(resolve, 1000));
        // isLoading.value = false;
    }

    const goPage = (path) => {
        router.push(path);
    }

    const selectedCity = (city) => {
        // console.log('city:', city);
        formData.city = city;
        // showCityPicker.value = false;
    }


</script>
<style scoped>
    .search-card{
        margin-bottom: 16px;
    }
    .city-tag{
        padding: 4px 8px;
        border-radius: 16px;
        font-size: 14px;
        background-color: #f7f8fa;
        color: #666;
        transition: all 0.3s ease-in-out;
    }
    .city-tag.active{
        background-color: #007AFF;
        color: #fff;
    }
</style>