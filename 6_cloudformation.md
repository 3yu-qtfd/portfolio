## CloudFormationでのAWS環境コード化

インフラのコード化・自動化はクラウドエンジニアに求められるスキル。 
コード化できれば誰でも同じ環境を作ることができ、パラメータ値を変えることで異なる複数の環境を素早く作成できる。 
ただし、パターン化できるところとできないところがあるため、どこまでをコード化・自動化できるか考える必要がある。 

### 行ったこと

CloudFormationを利用し、これまでに構築したAWS環境をコード化する。  

1. 全リソースのテンプレートファイル作成  
2. リソース単位に分割したテンプレートファイル作成  
3. VPC→セキュリティグループ→EC2→ALB→RDS→S3 の順でスタックを作成   
![CloudFormationスタック](images/lecture10/0_CloudFormation_stack.PNG)    

- EC2→ALBの順で構築する必要があるが、後から構築すべきALBのセキュリティグループIDをEC2構築時点で呼び出す必要があったため 
各リソースのセキュリティグループを1つのテンプレートファイルに分けて作成。  

### コード
cloudformation_templates_1

### 構築した環境
- VPC
![VPC](images/lecture10/1_VPC.PNG)  
![VPC_パブリックサブネット](images/6_cloudformation/2_VPC_PublicSubnet-1a.PNG)  
![VPC_パブリックサブネット](images/6_cloudformation/3_VPC_PrivateSubnet-1a.PNG)  
![VPC_プライベートサブネット](images/6_cloudformation/4_VPC_PublicSubnet-1c.PNG)  
![VPC_プライベートサブネット](images/6_cloudformation/5_VPC_PrivateSubnet-1c.PNG)  

- セキュリティグループ
![セキュリティグループ](images/6_cloudformation/6_SecurityGroup_all.PNG)  

- EC2
![EC2](images/6_cloudformation/7_EC2.PNG) 
![EC2セキュリティグループ](images/6_cloudformation/8_EC2_SecurityGroup.PNG)  

- ALB
![ALB](images/6_cloudformation/9_ALB.PNG)  
![ALBセキュリティグループ](images/6_cloudformation/10_ALB_SecurityGroup.PNG)  
![ALBセキュリティグループ](images/6_cloudformation/11_ALB_SecurityGroup.PNG)  

- RDS
![RDS](images/6_cloudformation/12_RDS.PNG) 
![RDS](images/6_cloudformation/13_RDS.PNG) 
![RDSセキュリティグループ](images/6_cloudformation/14_RDS_SecurityGroup.PNG)  
![RDSセキュリティグループ](images/6_cloudformation/15_RDS_SecurityGroup.PNG) 

- S3
![S3](images/6_cloudformation/16_S3.PNG) 
![S3バケットポリシー](images/6_cloudformation/17_S3_BucketPolicy.PNG) 

### メモ
- !Ref [論理ID]：同一テンプレートファイルにあるリソースの論理IDを指定すると、決められたID値などを返す。 
  VPCの論理IDを指定すると、VPC IDを返すなど。 

- !GetAtt [論理ID].[取得したいリソース情報]：同一テンプレートファイルにあるリソースの論理IDを指定すると、決められたリソース情報を返す。 
  セキュリティグループの論理ID.グループIDを指定すると、セキュリティグループIDを返すなど。 

- !ImportValue：異なるテンプレートファイルにあるリソースの論理IDを指定すると、Export元で指定した値を返す。 
  Export元でOutputsの指定が必要。  