[TOC]
##精典实例
public enum  TaskOperateEnum {
    START_TASK(1,"start","开启定时任务"),
    START_ONECE_TASK(2,"startOnece","执行一次定时任务"),
    STOP_TASK(3,"stop","暂停定时任务");
    public int code;
    public String order;
    private String info;

    TaskOperateEnum(int code, String order, String info) {
        this.code = code;
        this.order = order;
        this.info = info;
    }
    public static int getCodeByOrder(String order){
        if(StringUtils.isBlank(order))
            return 0;
        for(TaskOperateEnum taskOperateEnum : TaskOperateEnum.values()){
            if(order.equals(taskOperateEnum.order)){
                return taskOperateEnum.code;
            }
        }
        return 0;
    }
}

##实例一
```
package com.haiziwang.kwms.common.domain.enums;


public enum BillTypeEnum {
    SALE(1, "销售订单"),
    SHOP(2, "门店订单"),
    TRANSFER(3, "调拨订单"),
    BACK_TO_SUPPLIER(4, "退厂订单"),;
    private final Integer code;
    private final String desc;
    private BillTypeEnum(Integer code, String desc) {
        this.code = code;
        this.desc = desc;
    }
    public Integer getCode() {
        return code;
    }
    public String getDesc() {
        return desc;
    }
    /**
     * 通过code获取枚举
     *
     * @param code
     */
    public static BillTypeEnum getByCode(Integer code) {
        for (BillTypeEnum status : values()) {
            if (status.getCode().intValue() == code) {
                return status;
            }
        }
        return null;
    }
}
```