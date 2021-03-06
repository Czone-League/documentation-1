---
title:
keywords: sample homepage
sidebar: Others_zh
permalink: DDXF_zh.html
folder: doc_zh/Others
giturl: https://github.com/ontio/ontology-ddxf/blob/master/README_cn.md
---

[English](./DDXF_en) / 中文


<h1 align="center">去中心化数据交易应用框架</h1>
<p align="center" class="version">Version 0.7.0 </p>

## 概述

针对目前中心化数据交易所的痛点如：数据缓存、隐私数据未经用户授权、数据版权无法保护等问题，本体提出分布式数据管理协议ONT DATA，并基于此协议推出去中心化数据交易框架DDXF。本体生体应用开发者可以基于DDXF开发满足各种细分场景需求和各具特色的去中心化数据交易应用，并支持数据交易平台之间交易互通。

DDXF基于Ontology BlockChain，通过一致性账本、智能合约、密码学技术完美实现数字资产去中心化交易。DDXF提供一系列智能合约模板、交易组件和密码学组件，上层应用可以非常方便地实现版权控制、契约式数据分享等场景需求。

DDXF提供的主要功能包括：

* 数据资产化DataToken
* 数据交易智能合约eXchange Smart Contract
* 数据交易SDK
* 一系列密码学组件（如：数据水印）

## 数据资产化

DataToken 简称DToken，是将现实中的任何资产或者数据映射到本体区块链的合约内数字资产。对于所要交易的数据或链外资产，需要按照本体合约资产接口规范，定义好智能合约，在链上注册，以便于链上交易。

DataToken中包括元数据MetaData，MetaData是对于资产化数据的数据结构和约束的描述。

在实例化DataToken过程中，将结合密码学组件，如数据水印等等，用于数据交易的追溯和版权追踪。

## 分布式数据管理协议

本体提出分布式数据管理协议ONT DATA，该协议对实体之间的数据交易行为定义了一整套协议规范，支持去中心化的不同主体间的数据协同、交换及功能扩展。

为了保证交易双方的权益，在协议的交易流程中引入一个作为“担保人”的中间方，保证“一手交钱，一手交货”的结算过程。该中间方负责保管买方的资金，并根据最终交易结果将该资金转给卖方或退回给买方。因为中间方负责交易的最终结算，必须具备足够的公正性与安全性。依托于分布式账本运作的智能合约，具有公开且去中心化管理的特点，十分适合承担中间方的角色。


### 参与方角色

* **数据需求方**：需要数据的机构/企业/个人；
* **数据提供方**：提供数据的机构/企业/个人，数据可以是源数据，也可以是加工数据，数据提供需要完全满足当地政府的法律法规；
* **用户代理机构**：负责和用户交互，以满足数据交易环节中需要用户的授权，用户代理机构形式可以多样（可以是企业OA系统、互联网平台甚至仅是简单的短信网关），但需要完整实现应用协议框架中定义的用户授权协议；
* **数据所有者**：即数据主体，可以是机构/企业/个人。
* **去中心化数据交易所**：去中心化下数据交易所不涉及数字资产或数据资产的搬运，仅作为一个服务机构，主要工作包括：1、运营可视化的数据交易页面或社区 2、制定行业中的数据交易和交换标准，便于买卖双方以及交易参与方高效交易。各行各业数据交换标准会有很大差异性，所以将有各种不同类型的去中心化数据交易所。

> Note: 根据工作模式的选择，去中心化数据交易所并不是必须的参与方。



### 工作模式

去中心化数据交易根据不同场景有不同的工作模式，分为点对点模式和交易所参与模式。DAPP开发者可以了解以下工作模式，并选择合适的模式进行设计开发。

> Note: 流程中数据需求方、提供方、数据所有者均可以是多个参与方。

#### 点对点模式

点对点模式无需交易所，该模式比较适用于买卖双方比较明确，数据交换比较简单，或者交换标准已经建立，交易所marketplace无需参与或者轻度参与，比如说征信企业之间征信报告的交换。点对点模式下，交易参与方需要集成或使用SDK来实现交易流程。

![](http://on-img.com/chart_image/5a54d944e4b01acda595f66d.png)

* 准备事项

    1 交易参与方开通ONT ID。
    2 在交易请求发起之前，需求方首先向数据交易合约地址存入一笔资金。

* 交易请求，智能合约锁仓

    数据需求方通过区块链向提供方发送数据交易请求，该请求包括不限于：交易信息、ONT ID等。同时，调用XSC智能合约资金锁定接口，锁定需求方必需的交易费用。

* 用户授权

    数据提供方收到需求方的请求之后，访问用户代理，发起授权申请。此时，用户代理可以通过本体认证需求方的身份，并根据数据所有者事先提供的访问策略进行授权处理。如果所有者没有设置访问策略，用户代理通知其进行授权操作。如果未能获得的授权，则交易终止。 
* 上传数据

    数据提供方根据请求方支持的对称加密算法，生成一次性会话密钥，使用会话密钥加密交易的数据和数据特征值，将密文上传到第三方可信存储（比如去中心化存储IPFS）。

* 智能合约解锁和分润

    数据提供方调用XSC智能合约资产解锁接口，释放锁定资金，进行数据资产和资金的交割，同时根据智能合约规则进行多方分润。 数据资产DToken通过事件推送的方式推送给数据接受方。

* 收取数据，交易完成

    数据需求方接收到智能合约事件通知后，获取DToken，通过DToken从第三方可信存储获取数据，交易完成。



#### 交易所参与模式

很多情况下，交易所的参与能够更好地定义数据交换标准、以及与各参与方的接口标准，使得数据交易参与方更加方便快捷。在该模式下，协作流程如下：

![](http://on-img.com/chart_image/5a56fe50e4b05a8ff2f8e716.png)



## 开始使用DDXF

### 1 准备工作

* 注册ONT ID

    所有参与方需要首先注册ONT ID，我们提供多种SDK进行注册

* 注册数字资产账户
     
    在大部分数据交易中，实体数据或链外资产都将被数字化成为Token，要求所有参与方也需要开通数字资产账户，以便于进行交易和结算。

注册入口:
    
[>> JAVA SDK](https://github.com/ontio/ontology-java-sdk) 
 
 [>> TS SDK](https://github.com/ontio/ontology-ts-sdk)  

### 2 注册DataToken资产

本体官方或者第三方数据交易所来提供标准化的智能合约模板，数据需求方、数据提供方或者数据交易所本身选择模板进行定制化。

DataToken部署、发行、转移等操作，可以参考我们的智能合约指南，进入[本体智能合约开发指南](https://github.com/ontio/documentation/tree/master/smart-contract-tutorial)，了解如何使用智能合约。
    


### 3 开发和部署数据交易的智能合约XSC
   
使用智能合约作为数据交易的数字化信任中介，有很多好处，如透明、无法抵赖、无法篡改等优点。但智能合约的编写需要相对有经验的开发者进行，甚至需要评审者参与评审。幸运的是，官方将根据一些典型的数据交易场景定义了一些通用的智能合约模板，可以直接使用。

智能合约交易XSC需要记录用户授权编号、交易价格、参与方分润等等信息。这些信息一般由数据交易所指定（当然也可以由买方或者多方商定）。在典型的数据交易智能合约模板中，需要明确设计和定义以下参数和函数。

**参数设置**
```json
    {
        "deposit_address": "aefd726ac55f14cc0a4acdf3b1",
        "price_at_ont": "0.1",
        "price_at_ong": "0.5"
    }
```

**资金锁定**

该函数由数据需求方Data requester调用，也可以授权交易所来调用。
```
bool Lock(byte[] serial_no,  byte[33] user_ontid,  byte[33] buyer_ontid, int amount, byte[] buyer_sig);
```

**资产交割**

该函数成功后，将完成资产交割，一方面完成锁定金额的清算，一方面DataToken也同时交付到需求方。该函数一般由数据提供方，或者多参与方共同签名确定后调用。

```
bool Confirm(byte[] serial_no, byte[33] buyer, byte[33] user, byte[33] issuer, int seller_bounty, int issuer_bounty, byte[] buyer_sig
```

您可以进入[本体智能合约开发指南](https://github.com/ontio/documentation/tree/master/smart-contract-tutorial)，了解如何使用智能合约。

同时，DDXF已经准备好了基本的智能合约模板供您使用，进入[智能合约示例模板](https://github.com/ontio/ontology-ddxf/blob/master/demo/dex-sc-csharp/dex.cs)。

### 4 开始数据交易

如果以上工作已经完成，现在可以开始真正的数据交易了。

由于主要功能均基于智能合约实现，只要开发者已经完成第二步和第三步，即可使用SDK来操作智能合约交易。


