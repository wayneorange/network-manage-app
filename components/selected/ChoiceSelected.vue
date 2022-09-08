<template name="ChoiceSelected">
    <!-- 自定义下拉选择框 start-->
    <view class="selected-all">
        <view :class="isShowChoice ? 'drop-down-box-selected' : 'drop-down-box'" @click="btnShowHideClick">
            <text class="dropdown-content">{{choiceContent}}</text>
        </view>
        <!-- 弹框内容 -->
        <view class="dialog-view" v-if="isShowChoice">
            <text :class="choiceIndex ==index ?'dialog-title-selected':'dialog-title'"
                v-for="(item ,index) in choiceList" @click="btnChoiceClick(index)">{{item.choiceItemContent}}</text>
        </view>
    </view>
    <!-- 自定义下拉选择框 end -->
</template>

<script>
    export default {
        name: "ChoiceSelected",
        data() {
            return {
                isShowChoice: false
            };
        },
        props: {
            choiceIndex: {},
            choiceList: {},
            choiceContent: {},
        },
        methods: {
            // 选择
            btnChoiceClick: function(position) {
                var _this = this
                // _this.choiceIndex = position
                _this.isShowChoice = false
                // _this.choiceContent = _this.choiceList[position].choiceItemContent
                _this.$emit("onChoiceClick",position)
            },
            // 显示与隐藏选择内容
            btnShowHideClick: function() {
                var _this = this
                if (_this.isShowChoice) {
                    _this.isShowChoice = false  
                } else {
                    _this.isShowChoice = true
                }
            },
        }
    }
</script>

<style>
    /* end */
	/* 选择子标题的格式 */
    .dialog-title-selected {
        color: white;
        font-size: 30rpx;
        padding-left: 20rpx;
        padding-top: 6rpx;
        padding-bottom: 6rpx;
        padding-right: 15rpx;
        background-color: #c8c8c8; 
    }

    .dialog-title {
        color: black;
        font-size: 30rpx;
        padding-left: 20rpx;
        padding-top: 6rpx;
        padding-bottom: 6rpx;
        padding-right: 15rpx;
        background-color: white;
    }
	/* 下拉视图的表框格式 */
    .dialog-view {
        display: flex;
        flex-direction: column;
        justify-content: center;
        width: 100%;
        border: 2rpx solid #686868;
        box-sizing: border-box;
    }

    .dropdown-icon {
        width: 30rpx;
        height: 30rpx;
        margin-left: 15rpx;
        margin-right: 20rpx;
    }

    .dropdown-content {
        color: black;
        font-size: 30rpx;
        padding-left: 20rpx;
        padding-top: 5rpx;
        padding-bottom: 5rpx;
        flex-grow: 1;
    }
	/* 点击view视图的格式 */
    .drop-down-box-selected {
        display: flex;
        flex-direction: row;
        align-items: center;
        min-height: 60rpx;
        width: 100%;
        border: 2rpx solid #686868;
        box-sizing: border-box;
    }

    .drop-down-box {
        display: flex;
        flex-direction: row;
        align-items: center;
        min-height: 60rpx;
        width: 100%;
        border: 2rpx solid gray;
        border-radius: 5rpx;
        box-sizing: border-box;
    }

    .selected-all {
        display: flex;
        flex-direction: column;
        justify-content: center;
        width: 100%;
    }

    /* start */
</style>