---
title: iOS 允许http请求
urlname: sbb4v3ordg2vt41b
date: 2024-09-09T19:07:05.000Z
updated: '2024-09-12 15:03:46'
author: gaoyanchen
description: '---title: iOS 允许http请求date: 2024-09-09 19:07:05tags: "笔记"thumbnail: "https://www.apple.com.cn/newsroom/images/logos/quick-reads-logos/Apple-logo.jp...'
tags: 笔记
thumbnail: 'https://www.apple.com.cn/newsroom/images/logos/quick-reads-logos/Apple-logo.jpg.square_social.jpg'
---
以下两种情况允许 http 请求

## 允许个别域名 http 请求:
(1) NSAllowsArbitraryLoads: false,

(2) NSExceptionDomains内NSExceptionAllowsInsecureHTTPLoads: true

<key>NSAppTransportSecurity</key>

	<dict>

		<key>NSAllowsArbitraryLoads</key>

		<flase/>

		<key>NSExceptionDomains</key>

		<dict>

			<key>aliyuncs.com</key>

			<dict>

				<key>NSExceptionAllowsInsecureHTTPLoads</key>

				<true/>

			</dict>

		</dict>

	</dict>



## 允许所有 http 请求:
(1) NSAllowsArbitraryLoads: true

(2) 不设置NSExceptionDomains

<key>NSAppTransportSecurity</key>

	<dict>

		<key>NSAllowsArbitraryLoads</key>

		<true/>

	</dict>







