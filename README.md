### 复选框/带全选按钮的复选框/带二级全选按钮的复选框/带有两个全选按钮的二级全选按钮的复选框

https://github.com/sunguangqing/checkBox/blob/master/checkbox.html

### 复选框
#### `HTML结构：`
```HTML
<label class="checkedBox"><i></i><input class="checkedInput" type="checkbox" />记住密码</label>
```

#### `CSS代码：`
```CSS
/*勾选按钮*/
.checkedBox{
    cursor: pointer;
}
.checkedBox i{
    width: 16px;
    height: 16px;
    border: 1px solid #d9d9d9;
    -webkit-border-radius: 4px;
    -moz-border-radius: 4px;
    border-radius: 4px;;
    vertical-align: text-bottom;
}
.checkedBox i.checked{
    border: 0;
    background: url("../images/login-sprite.png") no-repeat -23px -42px;
}
.checkedBox input{
    opacity: 0;
    margin-left: -6px;
}
```

#### `JS代码：`
```javascript
$(".checkedInput").on("click", function () {
      var checked = $(this).prop("checked");
      if(checked){
          $(this).siblings("i").addClass("checked");
      }else {
          $(this).siblings("i").removeClass("checked");
      }
  });
```

### 带全选按钮的复选框
#### `JS代码：`
```javascript
// 全选
// 第一个参数是：全选按钮选择器， 第二个参数是：对应的所有子复选按钮的包裹选择器
function checkInputAll(selector, selbox) {
    $(selector).on("click", function(){
        var checked = $(this).prop("checked"),
            choice = $(selbox).find(".checkedInput");
        if(checked){
            $(this).siblings("i").addClass("checked");
            choice.prop("checked", true).siblings("i").addClass("checked");
        }else {
            $(this).siblings("i").removeClass("checked");
            choice.prop("checked", false).siblings("i").removeClass("checked");
        }
    });
}
checkInputAll(".checkedAll", ".checked-list_01");

// 单选
// 第一个参数是：所有子复选按钮的包裹选择器， 第二个参数是：对应的全选按钮选择器
function checkInputSolo(selector, selAll) {
    $(selector).on("click", function(){
        var checked = $(this).prop("checked");
        if(checked){
            $(this).siblings("i").addClass("checked");
        }else {
            $(this).siblings("i").removeClass("checked");
        }
        var checkedAll = [];
        $(selector).each(function(){
            var checked = $(this).prop("checked");
            checkedAll.push(checked);
            if(checkedAll.indexOf(false) !== -1){
                $(selAll).prop("checked", false).siblings("i").removeClass("checked");
            }else {
                $(selAll).prop("checked", true).siblings("i").addClass("checked");
            }
        });
    });
}
checkInputSolo(".checked-list_01 .checkedInput", ".checkedAll");
```

### 带二级全选按钮的复选框/带有两个全选按钮的二级全选按钮的复选框
#### `JS代码：`
```javascript
// 带有二级全选按钮的复选框
// 全选
// 第一个参数是：全选总选择器(一级全选按钮 二级全选按钮)
// 如果是一级全选,那么第二个参数必填,第三个参数不填；如果是二级全选,那么第二个参数不填,第三个参数必填  两种情况均需调用一次
// 第二个参数是：一级全选按钮下面的二级全选列表总包裹层
// 第三个参数是：每一个二级复选框列表的包裹选择器(包含二级全选按钮的包裹层)
// 第四个参数是：如果有两个全选按钮是 设置为true

// 调用时 根据第一个参数去判断第二个参数和第三个参数是否需要填写
function checkInputAll_02(selector, selbox, parents, bool) {
    $(selector).on("click", function(){
        var checked = $(this).prop("checked"),
            choice;
        if(selbox){
            choice= $(selbox).find(".checkedInput");
        }else {
            if(parents){
                choice= $(this).parents(parents).find(".checkedInput");
            }else {
                choice= $(this).find(".checkedInput");
            }
        }
        if(bool){
            if(checked){
                $(selector).prop("checked", true).siblings("i").addClass("checked");
                choice.prop("checked", true).siblings("i").addClass("checked").parents(".checked-list").addClass("active");
            }else {
                $(selector).prop("checked", false).siblings("i").removeClass("checked");
                choice.prop("checked", false).siblings("i").removeClass("checked").parents(".checked-list").removeClass("active");
            }
        }else {
            if(checked){
                $(this).siblings("i").addClass("checked");
                choice.prop("checked", true).siblings("i").addClass("checked").parents(".checked-list").addClass("active");
            }else {
                $(this).siblings("i").removeClass("checked");
                choice.prop("checked", false).siblings("i").removeClass("checked").parents(".checked-list").removeClass("active");
            }
        }
    });
}
checkInputAll_02(".checkedAll_2", ".checked-list_wrap_02", "");
checkInputAll_02(".checkedAll_list_02", "", ".checked-list_02");

checkInputAll_02(".checkedAll_3", ".checked-list_wrap_03", "", true);
checkInputAll_02(".checkedAll_list_03", "", ".checked-list_03");
// 带有全选按钮的复选框 用这第二个方法也可以实现
// checkInputAll_02(".checkedAll", ".checked-list_01");

// 单选
// 第一个参数是：二级全选复选框列表的包裹选择器(包括二级全选按钮在内的每一个单独包裹层)
// 第二个参数是：第三个参数的直接子复选按钮选择器和二级子复选按钮选择器   -- 两种情况都需调用一次
// 第三个参数是：全选按钮选择器   -- 如果是一级全选按钮，第一个参数不必填，如果是二级全选按钮，第一个参数必填

// 调用时 根据第三个参数去判断第一个参数是否需要填写
function checkInputSolo_02(parents, selector, selAll) {
    $(selector).on("click", function(){
        var checked = $(this).prop("checked");
        if(checked){
            $(this).siblings("i").addClass("checked");
            $(this).parents(".checked-list").addClass("active");
        }else {
            $(this).siblings("i").removeClass("checked");
            $(this).parents(".checked-list").removeClass("active");
        }
        var checkedAll = [],
            _parents;
        if(parents){
            _parents = $(this).parents(parents);
            $(_parents).find(selector).each(function(){
                var checked = $(this).prop("checked");
                checkedAll.push(checked);
                if(checkedAll.indexOf(false) !== -1){
                    _parents.find(selAll).prop("checked", false).siblings("i").removeClass("checked");
                }else {
                    _parents.find(selAll).prop("checked", true).siblings("i").addClass("checked");
                }
            });
        }else {
            $(selector).each(function () {
                var checked = $(this).prop("checked");
                checkedAll.push(checked);
                if (checkedAll.indexOf(false) !== -1) {
                    $(selAll).prop("checked", false).siblings("i").removeClass("checked");
                } else {
                    $(selAll).prop("checked", true).siblings("i").addClass("checked");
                }
            });
        }
    });
}
checkInputSolo_02("", ".list_02 .checkedInput", ".checkedAll_2");
checkInputSolo_02("", ".checkedAll_list_02", ".checkedAll_2");
checkInputSolo_02(".checked-list_02", ".list_02 .checkedInput", ".checkedAll_list_02");

checkInputSolo_02("", ".list_03 .checkedInput", ".checkedAll_3");
checkInputSolo_02("", ".checkedAll_list_03", ".checkedAll_3");
checkInputSolo_02(".checked-list_03", ".list_03 .checkedInput", ".checkedAll_list_03");
// 带有全选按钮的复选框  用这第二个方法也可以实现
// checkInputSolo_02("", ".checked-list_01 .checkedInput", ".checkedAll");
```
