---
title: 온-프레미스에서 Azure VMware 솔루션 (AVS) 액세스
description: 방화벽을 통해 온-프레미스 네트워크에서 Azure VMware 솔루션 (AVS)에 액세스
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/08/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 09b25dbdc8fc13c40ccd89b2cfd78611cedaac9d
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024469"
---
# <a name="accessing-your-avs-private-cloud-environment-and-applications-from-on-premises"></a>온-프레미스에서 AVS 사설 클라우드 환경 및 응용 프로그램에 액세스

Azure Express 경로 또는 사이트 간 VPN을 사용 하 여 온-프레미스 네트워크에서 AVS로 연결을 설정할 수 있습니다. AVS 사설 클라우드 vCenter 및 연결을 사용 하 여 AVS 사설 클라우드에서 실행 하는 워크 로드에 액세스 합니다. 온-프레미스 네트워크의 방화벽을 사용 하 여 연결에서 열리는 포트를 제어할 수 있습니다. 이 문서에서는 몇 가지 일반적인 응용 프로그램 포트 요구 사항에 대해 설명 합니다. 다른 응용 프로그램의 경우 포트 요구 사항에 대 한 응용 프로그램 설명서를 참조 하세요.

## <a name="ports-required-for-accessing-vcenter"></a>VCenter 액세스에 필요한 포트

AVS 사설 클라우드 vCenter 및 NSX manager에 액세스 하려면 아래 테이블에 정의 된 포트를 온-프레미스 방화벽에서 열어야 합니다. 

| Port       | 원본                           | 대상                      | 용도                                                                                                                |
|------------|----------------------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)   | 온-프레미스 DNS 서버          | AVS 사설 클라우드 DNS 서버        | Az의 DNS 조회를 전달 하는 데 필요 *합니다.* 온-프레미스 네트워크에서 AVS 사설 클라우드 DNS 서버로 AVS.io.     |
| 53 (UDP)   | AVS 사설 클라우드 DNS 서버        | 온-프레미스 DNS 서버          | DNS 전달에는 온-프레미스 도메인 이름을 AVS 사설 클라우드 vCenter에서 온-프레미스 DNS 서버로 조회 하는 데 필요 합니다. |
| 80 (TCP)   | 온-프레미스 네트워크              | AVS 사설 클라우드 관리 네트워크 | VCenter URL을 *http* 에서 *https*로 리디렉션하는 데 필요 합니다.                                                         |
| 443 (TCP)  | 온-프레미스 네트워크              | AVS 사설 클라우드 관리 네트워크 | 온-프레미스 네트워크에서 vCenter 및 NSX manager에 액세스 하는 데 필요 합니다.                                           |
| 8000 (TCP) | 온-프레미스 네트워크              | AVS 사설 클라우드 관리 네트워크 | 온-프레미스에서 AVS 사설 클라우드로 가상 머신을 vMotion 하는 데 필요 합니다.                                          |
| 8000 (TCP) | AVS 사설 클라우드 관리 네트워크 | 온-프레미스 네트워크              | AVS 사설 클라우드에서 온-프레미스로 가상 머신을 vMotion 하는 데 필요 합니다.                                          |

## <a name="ports-required-for-using-on-premises-active-directory-as-an-identity-source"></a>온-프레미스 active directory를 id 원본으로 사용 하는 데 필요한 포트

온-프레미스 active directory를 AVS 사설 클라우드 vCenter의 id 원본으로 구성 하려면 테이블에 정의 된 포트를 열어야 합니다. 구성 단계는 [Azure AD를 Azure AD를 사용 하 여 avs의 vCenter에 대 한 id 공급자로 사용](https://docs.azure.cloudsimple.com/azure-ad/) 을 참조 하세요.

| Port         | 원본                           | 대상                                         | 용도                                                                                                                                          |
|--------------|----------------------------------|-----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)      | AVS 사설 클라우드 DNS 서버        | 온-프레미스 DNS 서버                             | 온-프레미스 active directory 도메인 이름을 AVS 사설 클라우드 vCenter에서 온-프레미스 DNS 서버로 전달 하는 데 필요 합니다.        |
| 389(TCP/UDP) | AVS 사설 클라우드 관리 네트워크 | 온-프레미스 active directory 도메인 컨트롤러     | 사용자 인증을 위해 AVS 사설 클라우드 vCenter server에서 active directory 도메인 컨트롤러로의 LDAP 통신에 필요 합니다.              |
| 636 (TCP)     | AVS 사설 클라우드 관리 네트워크 | 온-프레미스 active directory 도메인 컨트롤러     | 사용자 인증을 위해 AVS 사설 클라우드 vCenter server에서 active directory 도메인 컨트롤러로의 보안 LDAP (LDAPS) 통신에 필요 합니다. |
| 3268 (TCP)    | AVS 사설 클라우드 관리 네트워크 | 온-프레미스 active directory 글로벌 카탈로그 서버 | 다중 도메인 컨트롤러 배포의 LDAP 통신에 필요 합니다.                                                                      |
| 3269 (TCP)    | AVS 사설 클라우드 관리 네트워크 | 온-프레미스 active directory 글로벌 카탈로그 서버 | 다중 도메인 컨트롤러 배포에서 LDAPS 통신에 필요 합니다.                                                                     |                                           |

## <a name="common-ports-required-for-accessing-workload-virtual-machines"></a>워크 로드 가상 컴퓨터에 액세스 하는 데 필요한 일반 포트

AVS 사설 클라우드에서 실행 되는 액세스 워크 로드 가상 머신에는 온-프레미스 방화벽에서 포트를 열어야 합니다. 아래 표에서는 필요한 공통 포트와 용도를 보여 줍니다. 응용 프로그램 관련 포트 요구 사항은 응용 프로그램 설명서를 참조 하세요.

| Port         | 원본                         | 대상                          | 용도                                                                              |
|--------------|--------------------------------|--------------------------------------|--------------------------------------------------------------------------------------|
| 22 (TCP)      | 온-프레미스 네트워크            | AVS 사설 클라우드 워크 로드 네트워크       | AVS 사설 클라우드에서 실행 되는 Linux 가상 머신에 대 한 보안 셸 액세스            |
| 3389 (TCP)    | 온-프레미스 네트워크            | AVS 사설 클라우드 워크 로드 네트워크       | AVS (AVS Private Cloud)에서 실행 되는 windows 가상 머신에 대 한 원격 데스크톱               |
| 80 (TCP)      | 온-프레미스 네트워크            | AVS 사설 클라우드 워크 로드 네트워크       | AVS 사설 클라우드에서 실행 되는 가상 컴퓨터에 배포 된 모든 웹 서버에 액세스 합니다.      |
| 443 (TCP)     | 온-프레미스 네트워크            | AVS 사설 클라우드 워크 로드 네트워크       | AVS 사설 클라우드에서 실행 되는 가상 컴퓨터에 배포 된 모든 보안 웹 서버에 액세스 합니다. |
| 389(TCP/UDP) | AVS 사설 클라우드 워크 로드 네트워크 | 온-프레미스 active directory 네트워크 | Windows 작업 가상 컴퓨터를 온-프레미스 active directory 도메인에 가입 시킵니다.     |
| 53 (UDP)      | AVS 사설 클라우드 워크 로드 네트워크 | 온-프레미스 네트워크                  | 온-프레미스 DNS 서버에 대 한 작업 가상 컴퓨터에 대 한 DNS 서비스 액세스       |

## <a name="next-steps"></a>다음 단계

* [Vlan 및 서브넷 만들기 및 관리](https://docs.azure.cloudsimple.com/create-vlan-subnet/)
* [Azure Express 경로를 사용 하 여 온-프레미스 네트워크에 연결](https://docs.azure.cloudsimple.com/on-premises-connection/)
* [온-프레미스에서 사이트 간 VPN 설정](https://docs.azure.cloudsimple.com/vpn-gateway/)
