https://ccpc.test.huawei.com/ccpcmd/services/dispatch/secured/CCPC/EN/ccpcnps/getSurveyV2/1

params:

```JSON
{
	"keyword": "",
	"channel": "100002",
	"systemid": "",
	"brand": "Apple",
	"countryCode": "",
	"cVer": "9.0.3.309",
	"os": "12.0",
	"appID": "23",
	"language": "zh",
	"times": "0",
	"imeiEnc": "",
	"firmware": "",
	"emuiVersion": "",
	"sn": "YDNGA18B04000305",
	"model": ""
}
```


Response

```JSON
{
	"firstTime": "2018-11-20 02:34:41", //每次请求的时间
	"reason": "success",
	"surveyID": "1533886961341", //每份不同语言的问卷ID都不同
	"surveyContent": {
		"endDesc": "Survey submitted. Thank you for participating.",
		"questions": [{
			"subTitle": "Willingness to recommend",
			"question": "How likely are you to recommend products of this brand to your friends and colleagues?",//问题题目
			"pictureUrl": "",
			"options": [{
				"name": "10",//问题选项
				"remark": "(Extremely likely)"//选项备注
			}, {
				"name": "9"
			}, {
				"name": "8"
			}, {
				"name": "7"
			}, {
				"name": "6"
			}, {
				"name": "5"
			}, {
				"name": "4"
			}, {
				"name": "3"
			}, {
				"name": "2"
			}, {
				"name": "1"
			}, {
				"name": "0",
				"remark": "(Not at all likely)"
			}],
			"id": "100001",//题目ID
			"type": "option"
		}, {
			"subTitle": "Product experience",
			"question": "Are you satisfied with your product? Please rate your level of satisfaction.",
			"pictureUrl": "",
			"options": [{
				"name": "10",
				"remark": "(Extremely satisfied)"
			}, {
				"name": "9"
			}, {
				"name": "8"
			}, {
				"name": "7"
			}, {
				"name": "6"
			}, {
				"name": "5"
			}, {
				"name": "4"
			}, {
				"name": "3"
			}, {
				"name": "2"
			}, {
				"name": "1"
			}, {
				"name": "0",
				"remark": "(Not at all satisfied)"
			}],
			"id": "200001",
			"type": "option"
		}, {
			"subTitle": "Product appearance",
			"question": "Are you satisfied with the appearance of your product? Please rate your level of satisfaction.",
			"pictureUrl": "",
			"options": [{
				"name": "10",
				"remark": "(Extremely satisfied)"
			}, {
				"name": "9"
			}, {
				"name": "8"
			}, {
				"name": "7"
			}, {
				"name": "6"
			}, {
				"name": "5"
			}, {
				"name": "4"
			}, {
				"name": "3"
			}, {
				"name": "2"
			}, {
				"name": "1"
			}, {
				"name": "0",
				"remark": "(Not at all satisfied)"
			}],
			"id": "230001",
			"type": "option"
		}, {
			"subTitle": "Product hardware",
			"question": "Are you satisfied with your product's hardware(e.g. battery life, heat generation, cameras, sound quality and effects, display, call quality, network experience, and positioning and navigation services)? Please rate your level of satisfaction.",
			"pictureUrl": "",
			"options": [{
				"name": "10",
				"remark": "(Extremely satisfied)"
			}, {
				"name": "9"
			}, {
				"name": "8"
			}, {
				"name": "7"
			}, {
				"name": "6"
			}, {
				"name": "5"
			}, {
				"name": "4"
			}, {
				"name": "3"
			}, {
				"name": "2"
			}, {
				"name": "1"
			}, {
				"name": "0",
				"remark": "(Not at all satisfied)"
			}],
			"id": "210001",
			"type": "option"
		}, {
			"subTitle": "Product software",
			"question": "Are you satisfied with your product's software(e.g. usability, camera features, home screen, settings, gallery, and responsiveness)? Please rate your level of satisfaction.",
			"pictureUrl": "",
			"options": [{
				"name": "10",
				"remark": "(Extremely satisfied)"
			}, {
				"name": "9"
			}, {
				"name": "8"
			}, {
				"name": "7"
			}, {
				"name": "6"
			}, {
				"name": "5"
			}, {
				"name": "4"
			}, {
				"name": "3"
			}, {
				"name": "2"
			}, {
				"name": "1"
			}, {
				"name": "0",
				"remark": "(Not at all satisfied)"
			}],
			"id": "220001",
			"type": "option"
		}, {
			"subTitle": "Purchase channel",
			"question": "Through which channel did you buy this product?",
			"pictureUrl": "",
			"options": [{
				"name": "Huawei Experience Store"
			}, {
				"name": "Retailer other than Huawei Experience Store"
			}, {
				"name": "VMALL(Huawei Online Store)"
			}, {
				"name": "Other e-commerce channel"
			}, {
				"nextId": "400002",
				"name": "Others(e.g. received as a gift)"
			}],
			"id": "300002",
			"type": "option"
		}, {
			"subTitle": "Purchase experience",
			"question": "Are you satisfied with your purchase experience? Please rate your level of satisfaction.",
			"pictureUrl": "",
			"options": [{
				"name": "10",
				"remark": "(Extremely satisfied)"
			}, {
				"name": "9"
			}, {
				"name": "8"
			}, {
				"name": "7"
			}, {
				"name": "6"
			}, {
				"name": "5"
			}, {
				"name": "4"
			}, {
				"name": "3"
			}, {
				"name": "2"
			}, {
				"name": "1"
			}, {
				"name": "0",
				"remark": "(Not at all satisfied)"
			}],
			"id": "300001",
			"type": "option"
		}, {
			"subTitle": "Service channel",
			"question": "Which kind of service have you received from Huawei most recently?",
			"pictureUrl": "",
			"options": [{
				"name": "Service Center Repair"
			}, {
				"name": "Postal Repair"
			}, {
				"name": "Hotline service"
			}, {
				"name": "Official website customer service"
			}, {
				"name": "Service in HiCare"
			}, {
				"nextId": "510001",//下一题跳转ID
				"name": "None"
			}],
			"id": "400002",
			"type": "option"
		}, {
			"subTitle": "Service experience",
			"question": "Are you satisfied with the service you received? Please rate your level of satisfaction.",
			"pictureUrl": "",
			"options": [{
				"name": "10",
				"remark": "(Extremely satisfied)"
			}, {
				"name": "9"
			}, {
				"name": "8"
			}, {
				"name": "7"
			}, {
				"name": "6"
			}, {
				"name": "5"
			}, {
				"name": "4"
			}, {
				"name": "3"
			}, {
				"name": "2"
			}, {
				"name": "1"
			}, {
				"name": "0",
				"remark": "(Not at all satisfied)"
			}],
			"id": "400001",
			"type": "option"
		}, {
			"subTitle": "Brand experience",
			"question": "Do you like this brand? How would you rate the brand?",
			"pictureUrl": "",
			"options": [{
				"name": "10",
				"remark": "(Really like)"
			}, {
				"name": "9"
			}, {
				"name": "8"
			}, {
				"name": "7"
			}, {
				"name": "6"
			}, {
				"name": "5"
			}, {
				"name": "4"
			}, {
				"name": "3"
			}, {
				"name": "2"
			}, {
				"name": "1"
			}, {
				"name": "0",
				"remark": "(Really dislike)"
			}],
			"id": "510001",
			"type": "option"
		}, {
			"subTitle": "Country",
			"question": "Which country did you buy this product?",
			"pictureUrl": "",
			"options": [{
				"name": "United States"
			}, {
				"name": "United Kingdom"
			}, {
				"name": "Australia"
			}, {
				"name": "United Arab Emirates"
			}, {
				"name": "South Africa"
			}, {
				"name": "Malaysia"
			}, {
				"name": "Philippines"
			}, {
				"name": "India"
			}, {
				"name": "Other Countries"
			}],
			"id": "999999",
			"type": "option"
		}, {
			"subTitle": "Feedback and suggestions",
			"question": "Please add any feedback that you believe can help improve our products and services in the future. Please do not include personal data(phone number, email address, etc.) in your feedback.",
			"pictureUrl": "",
			"id": "900004",
			"type": "essay",//填空题
			"required": "false"
		}],
		"title": "User Experience Survey ",
		"startDesc": "Thank you for choosing Huawei. In order to improve our products and services we would like to ask you a few simple questions. Your responses will help us to give you a better user experience."
	},
	"queryTimes": "0",
	"resCode": "0"
}
```

1. surveyID: 每个不同语言的问卷ID都不同
2. 有选择, 有填空
3. 有根据选择的内容跳转到不同的下一题


```JSON
{
	"firstTime": "2018-11-20 02:44:29",
	"reason": "success",
	"surveyID": "1533798873953",
	"surveyContent": {
		"endDesc": "问卷调查提交成功，非常感谢您的参与。",
		"questions": [{
			"subTitle": "整体印象#26",
			"question": "您对本产品的整体印象如何？请根据您的满意程度评分。",
			"pictureUrl": "http://resourcephs1.vmall.com:80/public/qstnSurvey/b0377fc6-1b1f-4c21-b48c-e89949f84783.png",
			"options": [{
				"name": "10分",
				"remark": "（非常满意）"
			}, {
				"name": "9分"
			}, {
				"name": "8分"
			}, {
				"name": "7分"
			}, {
				"name": "6分"
			}, {
				"name": "5分"
			}, {
				"name": "4分"
			}, {
				"name": "3分"
			}, {
				"name": "2分"
			}, {
				"name": "1分"
			}, {
				"name": "0分",
				"remark": "（非常不满意）"
			}],
			"id": "200001",
			"type": "option"
		}, {
			"subTitle": "推荐给亲友#26",
			"question": "您愿意将本产品推荐给亲戚朋友吗？请根据您的意愿程度评分。",
			"pictureUrl": "http://resourcephs1.vmall.com:80/public/qstnSurvey/b78916b9-7bf6-41fb-af79-c14b66456fd8.png",
			"options": [{
				"name": "10分",
				"remark": "（非常愿意）"
			}, {
				"name": "9分"
			}, {
				"name": "8分"
			}, {
				"name": "7分"
			}, {
				"name": "6分"
			}, {
				"name": "5分"
			}, {
				"name": "4分"
			}, {
				"name": "3分"
			}, {
				"name": "2分"
			}, {
				"name": "1分"
			}, {
				"name": "0分",
				"remark": "（非常不愿意）"
			}],
			"id": "100001",
			"type": "option"
		}, {
			"subTitle": "反馈与建议#26",
			"question": "请您给出打分的原因，我们将持续改进、提升产品的用户体验。",
			"pictureUrl": "http://resourcephs1.vmall.com:80/public/qstnSurvey/82127f55-c200-4def-91df-590d763f8eb2.png",
			"id": "900004",
			"type": "essay",
			"required": "false"
		}],
		"title": "用户体验改进问卷调查",
		"startDesc": "感谢您使用本产品。为了不断改进产品，给您提供更好的体验，我们诚意邀请您回答三个小问题。"
	},
	"queryTimes": "0",
	"resCode": "0"
}
```