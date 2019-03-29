---
layout:     post
title:      Element之Form
subtitle:   总在这个表单上遇到麻烦，索性整理下
date:       2019-03-30
author:     WeiXiao
header-img: img/post-bg-keybord.jpg
catalog: true
tags:
    - Vue
    - element-ui
    - Javascript
---

>之前老在这个表单上采坑，这两天也老是遇到后端小哥哥问我表单上的各种事，索性在这里整理记录下
>
>记录的开始，不但是个人的重新理解，亦也是个人思维与表达能力的锤炼
>
>博主现在还很菜，初入前端不久，各位看官若是看到，不足之处还请指教，评论区已经打开了哦！


# 常见的一些问题

	element的表单功能还是蛮强大的，平常需要的表单需要提交的组件几乎都有，再借助vue的双向数据绑定，对于表单的数据的收集可谓是太方便了。
	那么你是否遇到过这样的问题呢？
	
	1.表单的校验会出现英文的不能为空？
	2.表单的校验不起作用，单个不起作用，全局不起作用？
	3.想要指定某个字段的校验，错误信息提示同自定义校验出现再指定字段的下发？
	4.如何退出时清除表单数据，或者只清空错误的校验信息，避免下次进入污染表单？

# 解决方法

#### 表单校验出现英文

	首先你要不要对表单中的某一字段校验
	如果要，那么无论如何都不要直接再el-form-item中使用required

	<el-form-item required>
       <el-input></el-input>
    </el-form-item>
	
	此处的required就是罪魁祸首，我们并不需要ta，直接干掉
	
#### 表单校验不起作用

单个表单不起作用：

	确认以下几点是否正确,如下代码：

	<el-form :model="ruleForm" ref="ruleForm" :rules="rules">
		<el-form-item label="姓名" prop="name">
			<el-input v-model="ruleForm.name"></el-input>
		</el-form-item>
	<el-form>

	data(){
		retrun {
			ruleForm:{
				name:''
			},
			rules:{
				name:[
					{ required:true, message:'请输入姓名', trigger:'blur'}
					{ min:2, max:6, message: '长度为2-6', trigger:'change'}
				]
			}
		}
	}

	上述代码中：
		:rules绑定的名字是否正确；
		prop的属性名'name'是否与rules规则里的'name'以及v-model中ruleForm.name中的'name'一致
		prop属性只可以写在<el-form-itme>上才有效哦

	友情提示：
		有人就会问了，为啥prop的属性名要和v-model绑定中数据名一致呢，这是因为from表单校验你只扔一个prop进去，他知道使用什么规则校验，但是他不知道校验谁去啊，是不是？如果你名字相同了，我们就对号入座呗，我就知道校验规则校验谁是吧！有兴趣的可以看看源码，我呢也会在下个月写一个我们自己的element的form，到那一片我在详细给大家介绍一下吧！

表单最后一步提交校验不起作用：

	上校验代码：我猜你肯定是这样做的

	submit(formName){
		this.$refs[formName].validate(valid)=>{
			if(valid){
				alert('success');
			}else{
				alert('error');
			}
		}
	}

	那么诸位可以首先在校验前，输入一下this.$refs[formName]是否存在，可以点开看看。如果存在，一般情况下，可能是你的
		:model写错了，我常常误写为:modal,这不会报错，而且只全局校验不好使；
		自定义校验，一定要在校验方法里成功是callback()；
	如果不存在,看看单个校验是否正常，正常时可能是你的：
		ref的属性名有些问题，看看有没有写错；

#### 指定表单校验信息

	这个问题可以这样哦：
		
		<el-form-item label="姓名" :error="errorMsg_name">
			<el-input v-model="ruleForm.name"></el-input>
		</el-form-item>

	这里你就可以随意啦，错误信息直接赋值，但是，一定要注意校验成功手动将errorMsg_name = ''(置为空)

#### 清除表单数据或校验信息

彻底清除数据及校验信息(退出或进入时)：

	this.$refs[formName].resetFields();

只清除校验数据：

	this.$refs[formName].clearValidate();
	或者单独指定清掉谁
	this.$refs[formName].clearValidate('name');

##结语

	小小的一些梳理和记录，之后遇到会有补充，也请大家多提提意见！

	我们遇到的问题只是小小的一步，但每一小步都希望我们有所收获！

	我是WeiXiao，愿你的世界干净如新！

