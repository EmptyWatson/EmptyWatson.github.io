<template>
	<view class="content">
		<view class="roll">
			<view class="loading-data">
				<u-loading size="90" :show="dataLoading" mode="flower"></u-loading>
			</view>
			<u-row v-if="textContent.clss_list && (textContent.clss_list.length > 0)">
				<u-col span="2">
					<u-tag text="类别:" size="medium"></u-tag>
				</u-col>				
				<u-col span="9" style="padding-left: 0rpx; padding-right: 0rpx;">
					<u-row v-for="cls in textContent.clss_list"  v-bind:key="cls.info" justify="flex-start">
						<u-tag  :text="cls.info" mode="light" type="success" size="small"/>
						<u-tag  :text='cls.cls_idx' mode="light" type="info" shape="circleRight" size="small"/>
					</u-row>
				</u-col>
			</u-row>
			<u-row v-if="textContent.code">
				<u-col span="2" >
					<u-tag text="编码:" size="medium"></u-tag>
				</u-col>
				<u-col span="9">
					<u-tag  :text="textContent.code" mode="light" type="success" size="large"/>	
					<u-tag  :text='textContent.cls_idx' mode="light" type="info" shape="circleRight" size="large"/>
				</u-col>
			</u-row>
			<u-row v-if="textContent.name">
				<u-col span="2" >
					<u-tag text="名字:" size="medium"></u-tag>
				</u-col>
				<u-col span="9">
					<u-tag  :text="textContent.name" mode="dark" type="success" size="large"/>
				</u-col>
			</u-row>
			<u-row  class="u-font-13" v-if="textContent.gbm_code">
				<u-col span="2" v-if="textContent.gbm_code">
					<u-tag text="国标:" size="medium"></u-tag>
				</u-col>				
				<u-col span="9" >
					<u-tag  :text="textContent.gbm_code" mode="light" type="success" size="large"/>
				</u-col>
			</u-row>
			<u-cell-group>
				<u-row  v-if="textContent.desc && (textContent.desc.length > 0)" style="font-size:medium">
					<u-divider>描述</u-divider>
					<text style="padding-left: 5rpx; padding-right: 0rpx;">
						{{textContent.desc}}
					</text>
				</u-row>

				<u-row v-if="textContent.sub_cls_stats && (textContent.sub_cls_stats.length > 0)">
					<u-divider>本类统计</u-divider>
					<u-col offset="1" span="11" style="padding-left: 0rpx; padding-right: 0rpx;">
						<u-row v-for="sub in textContent.sub_cls_stats"  v-bind:key="sub.name" justify="flex-start">
							<u-tag :text="sub.name"  mode="light" type="info" shape="square" size="small"/>
							<u-tag :text="sub.cnt"  mode="dark" type="info" shape="circleRight" size="small"/>							
						</u-row>
					</u-col>
				</u-row>

				<u-row v-if="textContent.sub_list && (textContent.sub_list.length > 0)">
					<u-divider>子分类</u-divider>
					<u-col offset="1" span="11" style="padding-left: 0rpx; padding-right: 0rpx;">
						<u-row v-for="sub in textContent.sub_list"  v-bind:key="sub.info" justify="space-between">
							<u-tag  :text="sub.info"  mode="dark" type="info" shape="square" size="small"/>
						</u-row>
					</u-col>
				</u-row>

				<u-row style="font-size:29rpx"  v-if="textContent.tasks && (textContent.tasks.length > 0)" >
					<u-divider>工作任务</u-divider>
					<ol style="padding-left: 8rpx; padding-right: 0rpx;">
						<li style="list-style-type:none" v-for="task in textContent.tasks"  v-bind:key="task.idx">
							<text>{{task.idx }} .&nbsp; {{task.ctx}}</text>
						</li >
					</ol>
				</u-row>
				
				<u-row style="font-size:29rpx"  v-if="textContent.kndworkers && (textContent.kndworkers.length > 0)">
					<u-divider>包含但不限于下列工种</u-divider>
					<u-col offset="1" span="10" >
						<u-tag v-for="worker in textContent.kndworkers"  v-bind:key="worker" :text='worker' mode="dark" type="info" shape="circle" size="small" style="margin: 10rpx;"/>
					</u-col>
				</u-row>
			</u-cell-group>
		</view>

		<view class="control-grp">
			<view class="control-grp-wrap">
				<view class="cls-idx-tag-wrap" text-align="center">
					<u-tag class="cls-idx-tag"  :text='(curIdx+1).toString()+"/"+seqPos.length.toString()' mode="light" type="info" shape="circle" size="small"/>
				</view>
				<u-row gutter="15" justify="space-between">
					<u-col span="3">
						<button class="control-btn" @tap="prevItem">上条</button>
					</u-col>
					<u-col span="5">
						<button class="control-btn" @tap="showTree">显示列表</button>
					</u-col>
					<u-col span="3">
						<button class="control-btn" @tap="nextItem">下条</button>
					</u-col>
				</u-row>
			</view>
		</view>

		<view>
			<tki-tree ref="tkitree" :selectParent="selectParent" :multiple="multiple" :range="list" :foldAll="flod" rangeKey="name" @confirm="treeConfirm"
			@cancel="treeCancel"></tki-tree>
		</view>		
	</view>
</template>

<style>

.content{
	display: flex;
	display: -webkit-flex;
	flex-direction: column;
	width: 92%;
	height: calc(100vh - 26rpx);
	padding-left: 20rpx;
	padding-right: 20rpx;
}

.roll {
	flex: 1;
	width: 100%;
	height: 100%;
	overflow-y: scroll;
	height: auto;
}

.cls-idx-tag-wrap{
	display: flex;
	width: 100%;
	margin-bottom: 10rpx;
	flex-direction: row;
	justify-content: center;
	/*background-color: #909399;*/
	text-align: center;
	/*
	color: #FFFFFF;
	border-radius: 12rpx;
	*/
}

.cls-idx-tag{
	width: 30%;
}

.control-grp{
	width: 100%;
	height: 130rpx;
}

.control-grp-wrap {
	border-top: 1px dashed  #ebeef5;
	padding-top: 2rpx;
	width: 100%;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	padding-left: 20rpx;
}

.control-btn{
	height: 80rpx;
	line-height: 80rpx;
	width: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;	
	background-color: rgba(236,245,255, 0.4) ;
	color: #07BB07;
}

.u-row {
	margin-bottom: 10rpx;
}

.loading-data{
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

::-webkit-scrollbar{//scroll滚动条设置
        width: 0px; height: 0px;background-color: #fff;  
}

</style>

<script>
	import tkiTree from '@/components/tki-tree/tki-tree.vue';
	import saHoverMenu from '@/components/sa-hover-menu/sa-hover-menu.vue';
	import config from 'config.js'
	let testData = {}
	if(config.debug){
		 try
		 {
			testData = require('@/static/data/profession_struct.json');
		 }
		catch(err){
			console.log('require fail');
		}
	}
	//console.log("testData: " + JSON.stringify(testData));
	
	let globalStaticData = {}
	 try
	 {
		globalStaticData = require('@/static/data/profession_struct.json');
	 }
	catch(err){
		console.log('require /static/data/profession_struct.json fail');
	}
	//console.log("globalStaticData: " + JSON.stringify(globalStaticData));
	
    const g_visPath = ["bigclss", "midclss", "smlclss", "dtlclss"];
	const g_visPathName = ["大类", "中类", "小类", "细类"];
	
	function profStructData2MenuTree(stData){
	  let resData = []
	  for(let i=0 ; i < stData["bigclss"].length; ++i ){
		let bigcls = stData["bigclss"][i]
		let bigclsT = {"id":bigcls["code"], "name": bigcls["code"]+" "+bigcls["name"] , "children":[]}
		for(let j = 0;  j < bigcls["midclss"].length; ++j){
		let midcls = bigcls["midclss"][j]
		let midclsT = {"id":midcls["code"], "name": midcls["code"]+" "+midcls["name"] , "children":[]}
		for(let k=0; k < midcls["smlclss"].length; ++k){
		  let smlcls = midcls["smlclss"][k]
		  let smlclsT = {"id":smlcls["code"], "name":smlcls["code"]+" "+smlcls["name"] , "children":[]}
		  for(let l=0; l < smlcls["dtlclss"].length; ++l){
		  let dtlcls = smlcls["dtlclss"][l]
		  let dtlclsT = {"id":dtlcls["code"], "name":dtlcls["code"]+" "+dtlcls["name"] , "children":[]}
		  smlclsT["children"].push(dtlclsT)
		  }
		  midclsT["children"].push(smlclsT)
		}
		bigclsT["children"].push(midclsT)
		}
		resData.push(bigclsT)
	  }
	  return resData
	}

	function travelTree(data, rootdata, depth, idx, pos, func){
		let visPath = g_visPath
		if(depth  > visPath.length){
			return
		}
		//清空深度之后的位置
		for(let i = visPath.length; i > depth ; --i){
			pos[i-1] = -1
		}

		//设置对应的位置
		if(depth >= 1){
			pos[depth-1] = idx
			if(func){
				func(data, rootdata, depth, idx, pos)
			}
		}

		//访问子节点
		if(depth  < visPath.length)
		{
			for(let i = 0; i < data[visPath[depth]].length; ++i){
				travelTree(data[visPath[depth]][i], rootdata, depth+1, i, pos, func)
			}
		}
	}

	function getDataByPos(data, pos){
		let visPath = g_visPath
		let td = data
		for(let i = 0; i < visPath.length; ++i){
			if(pos[i] >= 0 ){
				td = td[visPath[i]][pos[i]];
			}
		}
		return td
	}

	function getSeqPos(rawData){
		let visPath = g_visPath
		let mdfyData = rawData;
		let seqPos  = [];
		let cls_cnt_stats   = [0, 0, 0, 0];//{"bigcls_cnt":0, "midcls_cnt":0, "smlcls_cnt":0, "dtlcls_cnt":0}
		let curPos  = [-1, -1, -1, -1]
		//遍历树
		//depth: 0 - 根，1- 大类 2-中类
		//data 当前的data
		//pdata 父data
		travelTree(mdfyData, mdfyData, 0, 0, curPos, function(data, rootdata, depth, idx, pos){
			let posTmp  = pos.concat();
			let cls_depth = depth-1
			cls_cnt_stats[cls_depth]++;
			data["pos"] = posTmp;
			data["idx"] = seqPos.length
			data["cls_idx"] = cls_cnt_stats[cls_depth];
			data["deep"] = cls_depth;

			if(typeof(data["sub_cls_stats"]) == "undefined"){
				data["sub_cls_stats"] = Array(g_visPath.length)
				for(let i = 0; i < g_visPath.length; ++i){
					data["sub_cls_stats"][i] = 0
				}
			}
			
			//当前类统计
			//++(data["sub_cls_stats"][cls_depth])
			//遍历父路径链,在对应的分类上增加值
			let lstData = rootdata
			for(let i = 0; i < cls_depth; ++i){
				lstData = lstData[visPath[i]][pos[i]]
				++(lstData["sub_cls_stats"][cls_depth])
			}

			seqPos.push(posTmp)
		})
		mdfyData["cls_cnt_stats"] = cls_cnt_stats;
		return [mdfyData, seqPos, cls_cnt_stats]
	}
	
	function getSubClsStats(data, td){
		let sub_cls_stats = []
		let visPath = g_visPath
		let cur_deep = td["deep"]
		for(let i = cur_deep + 1; i < visPath.length; ++i){
				let stats_info = {}
				stats_info["name"] = g_visPathName[i] + ": "
				stats_info["cnt"]  = td["sub_cls_stats"][i] + " 个"
				sub_cls_stats.push(stats_info)
		}
		return sub_cls_stats
		//return clssList.reverse()
	}

	function getClssList(data, pos){
		let clssList = []
		let visPath = g_visPath
		let td = data
		for(let i = 0; i < visPath.length; ++i){
			if(pos[i] >= 0 ){
				td = td[visPath[i]][pos[i]];
				let text = td["code"] + " " + td["name"];
				let cls_info = {}
				cls_info["info"] = text
				cls_info["cls_idx"] = td["cls_idx"].toString() + "/" + data["cls_cnt_stats"][i].toString();
				clssList.push(cls_info)
			}
		}
		if(clssList.length) clssList.pop()
		return Array.from(new Set(clssList))
		//return clssList.reverse()
	}

	function getSubList(data, sl){
		if(!sl) return;

		let subList = []
		for(let i = 0; i < sl.length; ++i){
			let text = sl[i]["code"] + " " + sl[i]["name"];
			let sub_info = {}
			sub_info["info"] = text
			sub_info["code"] = sl[i]["code"]
			sub_info["name"] = sl[i]["name"]
			sub_info["gbm_code"] = sl[i]["gbm_code"]
			subList.push(sub_info)
		}
		//去重
		return Array.from(new Set(subList))
	}


	function getSubListCalc(data, td){
		if(!td) return;

		let subList = []
		let pos  = td["pos"]
		let visPath = g_visPath

		for(let i = 0; i < visPath.length -1 ; ++i){
			//获取子类
			let nxi = i + 1;
			if(pos[nxi] == -1 ){
				let ntd = td[visPath[nxi]]
				for(let i = 0; i < ntd.length; ++i){
					let text = ntd[i]["code"] + " " + ntd[i]["name"];
					let sub_info = {}
					sub_info["info"] = text
					sub_info["code"] = ntd[i]["code"]
					sub_info["name"] = ntd[i]["name"]
					sub_info["gbm_code"] = ntd[i]["gbm_code"]
					subList.push(sub_info)
				}
				//去重
				return Array.from(new Set(subList))
			}
		}
		//去重
		return Array.from(new Set(subList))
	}
	
	function convToTextView(td, data){
		let textObj = {}
		textObj["code"] = td["code"]
		textObj["gbm_code"] = td["gbm_code"]
		textObj["name"] = td["name"]
		textObj["desc"] = td["desc"]
		textObj["cls_idx"] = td["cls_idx"].toString() + "/" + data["cls_cnt_stats"][td["deep"]].toString();

		//本类子类统计
		textObj["sub_cls_stats"] = getSubClsStats(data, td)

		//类别列表
		textObj["clss_list"] = getClssList(data, td["pos"]);

		textObj["sub_list"]  = getSubListCalc(data, td);//getSubList(data, td["sub_list"]);
		if("tasks" in td){
			textObj["tasks"] = []
			for(let j = 0; j < td["tasks"].length; ++j){
				let t = td["tasks"][j]
				let task = {}
				task["idx"] = t["idx"]
				task["ctx"] = t["ctx"]
				textObj["tasks"].push(task)
			}
		}

		if("kndworkers" in td){
			textObj["kndworkers"] = []
			for(let j = 0; j < td["kndworkers"].length; ++j){
				let t = td["kndworkers"][j]
				let kndwork = ""
				kndwork = t
				textObj["kndworkers"].push(kndwork)
			}
		}
		return textObj;
	}
	
	function getText(data, e){
		let textObj = {type:"class", code:"", gbm_code:"", name:"", desc:"", "cls_idx":"", tasks:[]}
		let curIdx  = 0
		let visPath = g_visPath
		if(e.length == 1){
			let tmpData = data
			//最后一个是小类
			for(let i=0; i <= e[0].parents.length; ++i){
				let tmpId;
				if(i == e[0].parents.length){
					tmpId = e[0].id
				}else{
					tmpId = e[0].parents[i].id
				}

				tmpData = tmpData[visPath[i]].find(function(ele){
					if(ele.code == tmpId){
						return true;
					}
					return false;
				})

				if(i == e[0].parents.length)
				{
					textObj = convToTextView(tmpData, data)
					curIdx = tmpData["idx"]
				}
			}
		}
		return [textObj, curIdx];
	}

	let init_data = {
		"_id": "struct_data",
		"bigclss": [{
			"page_code": "第一大类",
			"page_name": "党的机关、国家机关、群众团体",
			"name": "党的机关、国家机关、群众团体",
			"code": "1",
			"gbm_code": "GBM10000",
			"desc": "和社会组织、企事业单位负责人在中国共产党机关，国家机关，民主党派和工商联，人民团体和群众团体、社会组织及其工作机构，基层群众自治组织，企业、事业单位中担任领导职务并具有决策、管理权的人员。------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------",
			"pages": ["/home/watson/codes/imgs/works_imgs/职业分类大典(2015)电子版.pdf_90.jpg"],
			"sub_list": [],
			"midclss": [{
				"name": "中国共产党机关负责人",
				"code": "1-01",
				"gbm_code": "GBM10100",
				"desc": "在中国共产党中央和地方各级机关及其工作机构中，担任领导职务的人员。",
				"pages": [],
				"sub_list": [],
				"smlclss": [{
					"code": "1-01-00",
					"gbm_code": "GBM10100",
					"name": "中国共产党机关负责人",
					"desc": "",
					"sub_list": [],
					"dtlclss": [{
						"code": "1-01-00-00",
						"name": "中国共产党机关负责人",
						"desc": "在中国共产党中央和地方各级机关及其工作机构中，担任领导职务的人员。国家机关负责人在各级人民代表大会常务委员会、国家行政机关、人民政协及其工作机构，人民法院和人民检察院中，担任领导职务并具有决策、管理权的人员。该标识为国家标准编码。国家权力机关负责人在各级人民代表大会常务委员会及其工作机构中，担任领导职务并具有决策、管理权的人员。在各级人民代表大会常务委员会及其工作机构中，担任领导职务并具有决策、管理权的人员。主要工作任务：",
						"tasks": [{
							"idx": "1",
							"ctx": "常务委员会负责人主要处理常务委员会的重要日常工作；"
						}, {
							"idx": "2",
							"ctx": "工作机构负责人主要为常务委员会会议和人民代表大会会议服务。"
						}]
					}]
				}, {
					"code": "1-03-00",
					"gbm_code": "GBM10300",
					"name": "民主党派和工商联负责人",
					"desc": "",
					"sub_list": [],
					"dtlclss": [{
						"code": "1-03-00-01",
						"name": "民主党派负责人",
						"desc": "在中国国民党革命委员会、中国民主同盟、中国民主建国会、中国民主促进会、中国农工民主党、中国致公党、九三学社、台湾民主自治同盟各级组织机构中，担任领导职务并具有决策、管理权的人员。主要工作任务：",
						"tasks": [{
							"idx": "1",
							"ctx": "在中国共产党的领导下，根据中国共产党的路线、方针和重大政策，组织制定"
						}, {
							"idx": "2",
							"ctx": "贯彻执行“长期共存，互相监督，肝胆相照，荣厚与共”的方针，组织和领导"
						}, {
							"idx": "3",
							"ctx": "围绕国家的中心任务，组织和领导其成员开展为建设有中国特色社会主义服务"
						}, {
							"idx": "4",
							"ctx": "依照法律和各自政党的章程，独立自主地开展工作，反映其成员和所联系群众"
						}, {
							"idx": "5",
							"ctx": "加强各自政党的领导班子建设、思想建设和组织建设，开展思想政治工作，根"
						}],
						"kndworkers": ["细纱机操作工", "简并摇工", "热线工"]
					}, {
						"code": "1-03-00-02",
						"name": "工商联负责人",
						"desc": "在中华全国工商业联台会各级组织机构中，担任领导职务并具有决策、管理权的人员。主要工作任务：",
						"tasks": [{
							"idx": "1",
							"ctx": "参与国家大政方针及政治、经济、社会生活中的重要问题的政治协商；"
						}, {
							"idx": "2",
							"ctx": "引导会员积极参加国家经济建设；"
						}]
					}]
				}]
			}]
		}]
	};

	export default {
		components: {
			tkiTree
		},
		data() {
			if(Object.keys(globalStaticData).length > 0){
				init_data = globalStaticData;
			}
			if(config.debug && Object.keys(testData).length > 0 && !wx.cloud){
				init_data = testData;
			}

			return {
				list: [
					{
						id: 1,
						name: '初始类别',
					},
				],
				rawData: init_data,
				visPathName: g_visPathName,
				mdfyData: {},
				seqPos:[],
				multiple: false,
				selectParent: true,
				flod: false,
				show: true ,
				textContent: {type:"class", code:"", gbm_code:"", name:"", desc:"", tasks:""},
				btnList: [		// 所有按钮 
					{
						icon: '首',
						text: '平台首页',
						bgcolor: '#FE7A7D'
					},
				],
				scrollTop: 0,
				curSel:[0,-1,-1,-1],
				curIdx:0,
				dataLoading: true,
				cls_cnt_stats:[]
			}
		},
		onPageScroll(e) {
				this.scrollTop = e.scrollTop;
		},
		onShareAppMessage(res){
			//if (res.from === 'button') {// 来自页面内分享按钮
			//  console.log(res.target)
			//}
			let curText = this.getCurText()
			return {
			  title: '分享职业:' + curText["name"],
			  path: '/pages/index/index?idx=' + this.curIdx
			}
		},
		onShareTimeline(res){
			let curText = this.getCurText()
			return {
			  title: '分享职业:' + curText["name"],
			  path: '/pages/index/index?idx=' + this.curIdx
			}
		},
		onLoad(options) {
			console.log(options)
			let that = this
			let iniIdx = -1;//表示随机选取
			that.list =profStructData2MenuTree(that.rawData)
			// if(wx.cloud){

			// 	wx.showShareMenu({
			// 		withShareTicket: true,
			// 		menus: ['shareAppMessage', 'shareTimeline']
			// 	})

			// 	wx.cloud.downloadFile({
			// 	  fileID: 'cloud://job-class-cloud-1-5e21eh8997e7bb.6a6f-job-class-cloud-1-5e21eh8997e7bb-1304804109/profession_struct.json', // 文件 ID
			// 	  success: res => {
			// 		// 返回临时文件路径
			// 		console.log(res.tempFilePath)
			// 		const filePath = res.tempFilePath
			// 		const fs = wx.getFileSystemManager()
			// 		let result = fs.readFileSync(filePath,"utf-8")
			// 		let obj = JSON.parse(result);
			// 		//console.log(result)
			// 		//获取文件中的info信息
			// 		uni.setStorageSync('info', JSON.stringify(obj.info));
			// 		that.rawData = obj
			// 		that.list = profStructData2MenuTree(obj)
					
			// 		if(options.idx){
			// 			iniIdx = parseInt(options.idx)
			// 		}

			// 		that.initData(iniIdx)
			// 		that.dataLoading = false
			// 	  },
			// 	  fail: res => {
			// 		console.log(res)
			// 		that.dataLoading = false
			// 	  }
			// 	})				
			// 	// const db = wx.cloud.database();
			// 	// console.log('DataBase Init')
			// 	// const data = db.collection('profession_struct').where({
			// 	//   _id: 'struct_data'
			// 	// })
			// 	// .get()
			// 	// console.log('DataBase Get')
				
			// 	// data.then(res=>{
			// 	//   console.log(res)
			// 	//   that.rawData = res.data[0]
			// 	//   that.list = profStructData2MenuTree(res.data[0])
			// 	//   that.initData(iniIdx)
			// 	// })
			// }else
			{
				that.initData(iniIdx)
				that.dataLoading = false
			}
		},
		onShow(){
		},
		methods: {
			initData(iniIdx){
				let that = this
				let initCode = "";
				if(that.rawData["bigclss"].length){
					initCode = that.rawData["bigclss"][0]["code"];
				}
				let eTmp = [{id:initCode, parents:[]}]
				that.curTreeSel = eTmp;
				let md_sp = getSeqPos(that.rawData);
				that.mdfyData = md_sp[0]
				that.seqPos   = md_sp[1]
				that.cls_cnt_stats = md_sp[2]
				//console.log(md_sp[0])
				//console.log(md_sp[1])
				let rlt = getText(that.mdfyData, eTmp)
				that.textContent = rlt[0]
				that.curIdx = rlt[1]
				
				//随机选取一个
				if(that.seqPos && that.seqPos.length > 0){
					if( -1 == iniIdx ){
						let rndIdx = (Math.random() * that.seqPos.length) % that.seqPos.length;
						this.curIdx = Math.floor(rndIdx);
					}
					else if( iniIdx <= that.seqPos.length){
						this.curIdx = iniIdx
					}
					this.updateTextContent();
				}
			},
			getCurText(){
				let textObj = {}
				let td = getDataByPos(this.mdfyData, this.seqPos[this.curIdx])
				textObj = convToTextView(td, this.mdfyData)
				return textObj;
			},
			convertSelToEvent(sel){
				let e = [{id:0, parents:[]}];
				return e;
			},
			// 确定回调事件
			treeConfirm(e) {
				let that = this
				let rlt = getText(that.mdfyData, e);
				that.textContent = rlt[0]
				that.curIdx = rlt[1]
				console.log(e)
			},
			// 取消回调事件
			treeCancel(e) {
				console.log(e)
			},
			// 显示树形选择器
			showTree() {
				this.$refs.tkitree._show();
			},
			updateTextContent(){
				let curText = this.getCurText()
				this.$set(this.textContent, "code", curText["code"])
				this.$set(this.textContent, "name", curText["name"])
				this.$set(this.textContent, "gbm_code", curText["gbm_code"])
				this.$set(this.textContent, "desc", curText["desc"])
				this.$set(this.textContent, "clss_list", curText["clss_list"])
				this.$set(this.textContent, "sub_cls_stats", curText["sub_cls_stats"])
				this.$set(this.textContent, "sub_list", curText["sub_list"])
				this.$set(this.textContent, "cls_idx", curText["cls_idx"])
				this.$set(this.textContent, "tasks", curText["tasks"])
				this.$set(this.textContent, "kndworkers", curText["kndworkers"])
				//that.textContent = curText;
			},
			prevItem(){
				if(this.curIdx <= 0) this.curIdx = 0
				else --(this.curIdx)
				this.updateTextContent()
				console.log("prevItem")
				console.log(this.textContent)
			},
			nextItem(){
				if(this.curIdx >= (this.seqPos.length-1)) (this.seqPos.length-1)
				else ++(this.curIdx)
				this.updateTextContent()
				//console.log("nextItem")
				//console.log(this.textContent)
			},
			// 点击按钮 
			clickBtn: function(btn) {
				console.log('点击了2：' + btn.text);
			},			
		}
	}
</script>
