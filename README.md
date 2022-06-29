# Supplier-Api
  - [Context](#context)
  - [Requirement](#requirement)
  - [Solution](#solution)
    - [1. Use CDS Views and SEGW Project to generate OData API](#1-use-cds-views-and-segw-project-to-generate-odata-api)
    - [2. Use CDS Annotation @OData.publish](#2-use-cds-annotation-odatapublish)
## Context
All the supplier data is in the S4/HANA standard tables

## Requirement
1. Develop and expose a SAP API to non-SAP consumer platforms

## Solution

### 1. Use CDS Views and SEGW Project to generate OData API

In this method we will try to re-use released CDS view with respect to supplier from SAP and code as low as possible to generate a read only API.

A step by step guidelines on developing the above mentioned requirement.

1. Identify the supplier table "lfa1".
2. Search for a released CDS view from SAP for a supplier table in SAP Business Hub.
3. I_Supplier CDS view is released by CDS and is available for consumption.
4. Similarly search for corresponding supplier dependent relationship tables(lfb1, lfm1) released CDS views. Available released views from SAP are I_SupplierCompany and I_SupplierPurchasingOrg.
5. Create a consumption views for each released SAP CDS views for designing an APi. In our assignment we have created and modelled this consumption views as [ZA_SupplierApi](../Supplier-Api/CDS/DDL/ZA_SupplierApi.txt), [ZA_SupplierCompanyApi](../Supplier-Api/CDS/DDL/ZA_SupplierCompanyApi.txt), [ZA_SupplierPurchasingOrgApi](../Supplier-Api/CDS/DDL/ZA_SupplierPurchasingOrgApi.txt) and [ZA_SupplierTextApi](../Supplier-Api/CDS/DDL/ZA_SupplierTextApi.txt).
6. Activate all the CDS views
7. Create SEGW project using the Tcode: segw.
   
![SEGW_CREATE](CDS/Segw%20Project/img/segw_create_project.png)

1. Use the RDS approach and map the created CDS view [ZA_SupplierApi](../Supplier-Api/CDS/DDL/ZA_SupplierApi.txt).

![SEGW_MAP](CDS/Segw%20Project/img/segw_map_cds_view_as_reference.png)

9. Generate runtime objects for the SEGW project.

![GENERATE_RUNTIME](CDS/Segw%20Project/img/generate_runtime_artifacts.png)

10.  Register SEGW project as an OData service in "Activate and Maintain Services" GUI using T Code: /IWFND/MAINT_SERVICE
11.  Add SEGW Project as an OData service.

![Add service](CDS/Segw%20Project/img/register_segw_project_as_odata.png)

![Activate](Segw%20Project/odata_service_create_success.png)

12. Access the service in "Activate & Maintain Service" GUI.

![Maintain](CDS/Segw%20Project/img/activate_service.png)

12. Launch the Gateway client to test the OData service.

![Metadata Test](CDS/Segw%20Project/img/test_metadata_call_gw_client.png)

![Entity Test](CDS/Segw%20Project/img/test_supplier_api_entity.png)

13. A succesful run of API as $metadata call.

```
<?xml version="1.0" encoding="utf-8"?>
<app:service xml:lang="en" xml:base="https://ldai1uyt.wdf.sap.corp:44300/sap/opu/odata/sap/ZMIN_ASSIGNMENT_SUPPLIER_SRV/"
    xmlns:app="http://www.w3.org/2007/app"
    xmlns:atom="http://www.w3.org/2005/Atom"
    xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
    xmlns:sap="http://www.sap.com/Protocols/SAPData">
    <app:workspace>
        <atom:title type="text">Data</atom:title>
        <app:collection sap:creatable="false" sap:updatable="false" sap:deletable="false" sap:content-version="1" href="ZA_SupplierApi">
            <atom:title type="text">ZA_SupplierApi</atom:title>
            <sap:member-title>Supplier</sap:member-title>
        </app:collection>
        <app:collection sap:creatable="false" sap:updatable="false" sap:deletable="false" sap:content-version="1" href="ZA_SupplierCompanyApi">
            <atom:title type="text">ZA_SupplierCompanyApi</atom:title>
            <sap:member-title>Supplier Company</sap:member-title>
        </app:collection>
        <app:collection sap:creatable="false" sap:updatable="false" sap:deletable="false" sap:content-version="1" href="ZA_SupplierPurchasingOrgApi">
            <atom:title type="text">ZA_SupplierPurchasingOrgApi</atom:title>
            <sap:member-title>Supplier Purchasing Organization</sap:member-title>
        </app:collection>
        <app:collection sap:creatable="false" sap:updatable="false" sap:deletable="false" sap:content-version="1" href="ZA_SupplierTextApi">
            <atom:title type="text">ZA_SupplierTextApi</atom:title>
            <sap:member-title>Supplier Text</sap:member-title>
        </app:collection>
    </app:workspace>
    <atom:link rel="self" href="https://ldai1uyt.wdf.sap.corp:44300/sap/opu/odata/sap/ZMIN_ASSIGNMENT_SUPPLIER_SRV/"/>
    <atom:link rel="latest-version" href="https://ldai1uyt.wdf.sap.corp:44300/sap/opu/odata/sap/ZMIN_ASSIGNMENT_SUPPLIER_SRV/"/>
</app:service>

```
14. A succesful run of Supplier API returns the below response.

```
<feed xmlns="http://www.w3.org/2005/Atom"
    xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
    xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xml:base="https://ldai1uyt.wdf.sap.corp:44300/sap/opu/odata/sap/ZMIN_ASSIGNMENT_SUPPLIER_SRV/">
    <id>https://ldai1uyt.wdf.sap.corp:44300/sap/opu/odata/sap/ZMIN_ASSIGNMENT_SUPPLIER_SRV/ZA_SupplierApi</id>
    <title type="text">ZA_SupplierApi</title>
    <updated>2022-06-29T18:00:32Z</updated>
    <author>
        <name/>
    </author>
    <link href="ZA_SupplierApi" rel="self" title="ZA_SupplierApi"/>
    <entry>
        <id>https://ldai1uyt.wdf.sap.corp:44300/sap/opu/odata/sap/ZMIN_ASSIGNMENT_SUPPLIER_SRV/ZA_SupplierApi('%24000000001')</id>
        <title type="text">ZA_SupplierApi('%24000000001')</title>
        <updated>2022-06-29T18:00:32Z</updated>
        <category term="ZMIN_ASSIGNMENT_SUPPLIER_SRV.ZA_SupplierApiType" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme"/>
        <link href="ZA_SupplierApi('%24000000001')" rel="self" title="ZA_SupplierApiType"/>
        <link href="ZA_SupplierApi('%24000000001')/to_SupplierCompany" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierCompany" type="application/atom+xml;type=feed" title="to_SupplierCompany"/>
        <link href="ZA_SupplierApi('%24000000001')/to_SupplierPurchasingOrg" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierPurchasingOrg" type="application/atom+xml;type=feed" title="to_SupplierPurchasingOrg"/>
        <link href="ZA_SupplierApi('%24000000001')/to_SupplierText" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierText" type="application/atom+xml;type=feed" title="to_SupplierText"/>
        <content type="application/xml">
            <m:properties xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
                xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices">
                <d:Supplier>$000000001</d:Supplier>
                <d:AlternativePayeeAccountNumber></d:AlternativePayeeAccountNumber>
                <d:AuthorizationGroup></d:AuthorizationGroup>
                <d:CreatedByUser>C5314079</d:CreatedByUser>
                <d:CreationDate>2021-03-09T00:00:00</d:CreationDate>
                <d:Customer>3007</d:Customer>
                <d:PaymentIsBlockedForSupplier>false</d:PaymentIsBlockedForSupplier>
                <d:PostingIsBlocked>false</d:PostingIsBlocked>
                <d:PurchasingIsBlocked>false</d:PurchasingIsBlocked>
                <d:SupplierAccountGroup>KRED</d:SupplierAccountGroup>
                <d:SupplierFullName>Company INLANDSLIEFERANT/411046 PUNE</d:SupplierFullName>
                <d:SupplierName>INLANDSLIEFERANT</d:SupplierName>
                <d:VATRegistration></d:VATRegistration>
                <d:BirthDate m:null="true"/>
                <d:ConcatenatedInternationalLocNo>0000000 &amp;00000 &amp;0</d:ConcatenatedInternationalLocNo>
                <d:DeletionIndicator>false</d:DeletionIndicator>
                <d:FiscalAddress></d:FiscalAddress>
                <d:Industry></d:Industry>
                <d:InternationalLocationNumber1>0000000</d:InternationalLocationNumber1>
                <d:InternationalLocationNumber2>00000</d:InternationalLocationNumber2>
                <d:InternationalLocationNumber3>0</d:InternationalLocationNumber3>
                <d:IsNaturalPerson></d:IsNaturalPerson>
                <d:ResponsibleType></d:ResponsibleType>
                <d:SuplrQltyInProcmtCertfnValidTo m:null="true"/>
                <d:SuplrQualityManagementSystem></d:SuplrQualityManagementSystem>
                <d:SupplierCorporateGroup></d:SupplierCorporateGroup>
                <d:SupplierProcurementBlock></d:SupplierProcurementBlock>
                <d:TaxNumber1></d:TaxNumber1>
                <d:TaxNumber2></d:TaxNumber2>
                <d:TaxNumber3></d:TaxNumber3>
                <d:TaxNumber4></d:TaxNumber4>
                <d:TaxNumber5></d:TaxNumber5>
                <d:TaxNumberResponsible></d:TaxNumberResponsible>
                <d:TaxNumberType></d:TaxNumberType>
                <d:SuplrProofOfDelivRlvtCode></d:SuplrProofOfDelivRlvtCode>
                <d:BR_TaxIsSplit>false</d:BR_TaxIsSplit>
                <d:DataExchangeInstructionKey></d:DataExchangeInstructionKey>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://ldai1uyt.wdf.sap.corp:44300/sap/opu/odata/sap/ZMIN_ASSIGNMENT_SUPPLIER_SRV/ZA_SupplierApi('%24000000002')</id>
        <title type="text">ZA_SupplierApi('%24000000002')</title>
        <updated>2022-06-29T18:00:32Z</updated>
        <category term="ZMIN_ASSIGNMENT_SUPPLIER_SRV.ZA_SupplierApiType" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme"/>
        <link href="ZA_SupplierApi('%24000000002')" rel="self" title="ZA_SupplierApiType"/>
        <link href="ZA_SupplierApi('%24000000002')/to_SupplierCompany" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierCompany" type="application/atom+xml;type=feed" title="to_SupplierCompany"/>
        <link href="ZA_SupplierApi('%24000000002')/to_SupplierPurchasingOrg" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierPurchasingOrg" type="application/atom+xml;type=feed" title="to_SupplierPurchasingOrg"/>
        <link href="ZA_SupplierApi('%24000000002')/to_SupplierText" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierText" type="application/atom+xml;type=feed" title="to_SupplierText"/>
        <content type="application/xml">
            <m:properties xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
                xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices">
                <d:Supplier>$000000002</d:Supplier>
                <d:AlternativePayeeAccountNumber></d:AlternativePayeeAccountNumber>
                <d:AuthorizationGroup></d:AuthorizationGroup>
                <d:CreatedByUser>C5314079</d:CreatedByUser>
                <d:CreationDate>2021-05-19T00:00:00</d:CreationDate>
                <d:Customer></d:Customer>
                <d:PaymentIsBlockedForSupplier>false</d:PaymentIsBlockedForSupplier>
                <d:PostingIsBlocked>false</d:PostingIsBlocked>
                <d:PurchasingIsBlocked>false</d:PurchasingIsBlocked>
                <d:SupplierAccountGroup>KRED</d:SupplierAccountGroup>
                <d:SupplierFullName>Company INLANDSLIEFERANT/411046 PUNE</d:SupplierFullName>
                <d:SupplierName>INLANDSLIEFERANT</d:SupplierName>
                <d:VATRegistration></d:VATRegistration>
                <d:BirthDate m:null="true"/>
                <d:ConcatenatedInternationalLocNo>0000000 &amp;00000 &amp;0</d:ConcatenatedInternationalLocNo>
                <d:DeletionIndicator>false</d:DeletionIndicator>
                <d:FiscalAddress></d:FiscalAddress>
                <d:Industry></d:Industry>
                <d:InternationalLocationNumber1>0000000</d:InternationalLocationNumber1>
                <d:InternationalLocationNumber2>00000</d:InternationalLocationNumber2>
                <d:InternationalLocationNumber3>0</d:InternationalLocationNumber3>
                <d:IsNaturalPerson></d:IsNaturalPerson>
                <d:ResponsibleType></d:ResponsibleType>
                <d:SuplrQltyInProcmtCertfnValidTo m:null="true"/>
                <d:SuplrQualityManagementSystem></d:SuplrQualityManagementSystem>
                <d:SupplierCorporateGroup></d:SupplierCorporateGroup>
                <d:SupplierProcurementBlock></d:SupplierProcurementBlock>
                <d:TaxNumber1></d:TaxNumber1>
                <d:TaxNumber2></d:TaxNumber2>
                <d:TaxNumber3></d:TaxNumber3>
                <d:TaxNumber4></d:TaxNumber4>
                <d:TaxNumber5></d:TaxNumber5>
                <d:TaxNumberResponsible></d:TaxNumberResponsible>
                <d:TaxNumberType></d:TaxNumberType>
                <d:SuplrProofOfDelivRlvtCode></d:SuplrProofOfDelivRlvtCode>
                <d:BR_TaxIsSplit>false</d:BR_TaxIsSplit>
                <d:DataExchangeInstructionKey></d:DataExchangeInstructionKey>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://ldai1uyt.wdf.sap.corp:44300/sap/opu/odata/sap/ZMIN_ASSIGNMENT_SUPPLIER_SRV/ZA_SupplierApi('%24000000003')</id>
        <title type="text">ZA_SupplierApi('%24000000003')</title>
        <updated>2022-06-29T18:00:32Z</updated>
        <category term="ZMIN_ASSIGNMENT_SUPPLIER_SRV.ZA_SupplierApiType" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme"/>
        <link href="ZA_SupplierApi('%24000000003')" rel="self" title="ZA_SupplierApiType"/>
        <link href="ZA_SupplierApi('%24000000003')/to_SupplierCompany" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierCompany" type="application/atom+xml;type=feed" title="to_SupplierCompany"/>
        <link href="ZA_SupplierApi('%24000000003')/to_SupplierPurchasingOrg" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierPurchasingOrg" type="application/atom+xml;type=feed" title="to_SupplierPurchasingOrg"/>
        <link href="ZA_SupplierApi('%24000000003')/to_SupplierText" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierText" type="application/atom+xml;type=feed" title="to_SupplierText"/>
        <content type="application/xml">
            <m:properties xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
                xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices">
                <d:Supplier>$000000003</d:Supplier>
                <d:AlternativePayeeAccountNumber></d:AlternativePayeeAccountNumber>
                <d:AuthorizationGroup></d:AuthorizationGroup>
                <d:CreatedByUser>C5314033</d:CreatedByUser>
                <d:CreationDate>2021-03-09T00:00:00</d:CreationDate>
                <d:Customer></d:Customer>
                <d:PaymentIsBlockedForSupplier>false</d:PaymentIsBlockedForSupplier>
                <d:PostingIsBlocked>false</d:PostingIsBlocked>
                <d:PurchasingIsBlocked>false</d:PurchasingIsBlocked>
                <d:SupplierAccountGroup>KRED</d:SupplierAccountGroup>
                <d:SupplierFullName>Company INLANDSLIEFERANT/411046 PUNE</d:SupplierFullName>
                <d:SupplierName>INLANDSLIEFERANT</d:SupplierName>
                <d:VATRegistration></d:VATRegistration>
                <d:BirthDate m:null="true"/>
                <d:ConcatenatedInternationalLocNo>0000000 &amp;00000 &amp;0</d:ConcatenatedInternationalLocNo>
                <d:DeletionIndicator>false</d:DeletionIndicator>
                <d:FiscalAddress></d:FiscalAddress>
                <d:Industry></d:Industry>
                <d:InternationalLocationNumber1>0000000</d:InternationalLocationNumber1>
                <d:InternationalLocationNumber2>00000</d:InternationalLocationNumber2>
                <d:InternationalLocationNumber3>0</d:InternationalLocationNumber3>
                <d:IsNaturalPerson></d:IsNaturalPerson>
                <d:ResponsibleType></d:ResponsibleType>
                <d:SuplrQltyInProcmtCertfnValidTo m:null="true"/>
                <d:SuplrQualityManagementSystem></d:SuplrQualityManagementSystem>
                <d:SupplierCorporateGroup></d:SupplierCorporateGroup>
                <d:SupplierProcurementBlock></d:SupplierProcurementBlock>
                <d:TaxNumber1></d:TaxNumber1>
                <d:TaxNumber2></d:TaxNumber2>
                <d:TaxNumber3></d:TaxNumber3>
                <d:TaxNumber4></d:TaxNumber4>
                <d:TaxNumber5></d:TaxNumber5>
                <d:TaxNumberResponsible></d:TaxNumberResponsible>
                <d:TaxNumberType></d:TaxNumberType>
                <d:SuplrProofOfDelivRlvtCode></d:SuplrProofOfDelivRlvtCode>
                <d:BR_TaxIsSplit>false</d:BR_TaxIsSplit>
                <d:DataExchangeInstructionKey></d:DataExchangeInstructionKey>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://ldai1uyt.wdf.sap.corp:44300/sap/opu/odata/sap/ZMIN_ASSIGNMENT_SUPPLIER_SRV/ZA_SupplierApi('0')</id>
        <title type="text">ZA_SupplierApi('0')</title>
        <updated>2022-06-29T18:00:32Z</updated>
        <category term="ZMIN_ASSIGNMENT_SUPPLIER_SRV.ZA_SupplierApiType" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme"/>
        <link href="ZA_SupplierApi('0')" rel="self" title="ZA_SupplierApiType"/>
        <link href="ZA_SupplierApi('0')/to_SupplierCompany" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierCompany" type="application/atom+xml;type=feed" title="to_SupplierCompany"/>
        <link href="ZA_SupplierApi('0')/to_SupplierPurchasingOrg" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierPurchasingOrg" type="application/atom+xml;type=feed" title="to_SupplierPurchasingOrg"/>
        <link href="ZA_SupplierApi('0')/to_SupplierText" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierText" type="application/atom+xml;type=feed" title="to_SupplierText"/>
        <content type="application/xml">
            <m:properties xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
                xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices">
                <d:Supplier>0</d:Supplier>
                <d:AlternativePayeeAccountNumber></d:AlternativePayeeAccountNumber>
                <d:AuthorizationGroup></d:AuthorizationGroup>
                <d:CreatedByUser>C5314079</d:CreatedByUser>
                <d:CreationDate>2021-02-24T00:00:00</d:CreationDate>
                <d:Customer></d:Customer>
                <d:PaymentIsBlockedForSupplier>false</d:PaymentIsBlockedForSupplier>
                <d:PostingIsBlocked>false</d:PostingIsBlocked>
                <d:PurchasingIsBlocked>false</d:PurchasingIsBlocked>
                <d:SupplierAccountGroup>KRED</d:SupplierAccountGroup>
                <d:SupplierFullName>Mr./700030</d:SupplierFullName>
                <d:SupplierName></d:SupplierName>
                <d:VATRegistration></d:VATRegistration>
                <d:BirthDate m:null="true"/>
                <d:ConcatenatedInternationalLocNo>0000000 &amp;00000 &amp;0</d:ConcatenatedInternationalLocNo>
                <d:DeletionIndicator>false</d:DeletionIndicator>
                <d:FiscalAddress></d:FiscalAddress>
                <d:Industry></d:Industry>
                <d:InternationalLocationNumber1>0000000</d:InternationalLocationNumber1>
                <d:InternationalLocationNumber2>00000</d:InternationalLocationNumber2>
                <d:InternationalLocationNumber3>0</d:InternationalLocationNumber3>
                <d:IsNaturalPerson></d:IsNaturalPerson>
                <d:ResponsibleType></d:ResponsibleType>
                <d:SuplrQltyInProcmtCertfnValidTo m:null="true"/>
                <d:SuplrQualityManagementSystem></d:SuplrQualityManagementSystem>
                <d:SupplierCorporateGroup></d:SupplierCorporateGroup>
                <d:SupplierProcurementBlock></d:SupplierProcurementBlock>
                <d:TaxNumber1></d:TaxNumber1>
                <d:TaxNumber2></d:TaxNumber2>
                <d:TaxNumber3></d:TaxNumber3>
                <d:TaxNumber4></d:TaxNumber4>
                <d:TaxNumber5></d:TaxNumber5>
                <d:TaxNumberResponsible></d:TaxNumberResponsible>
                <d:TaxNumberType></d:TaxNumberType>
                <d:SuplrProofOfDelivRlvtCode></d:SuplrProofOfDelivRlvtCode>
                <d:BR_TaxIsSplit>false</d:BR_TaxIsSplit>
                <d:DataExchangeInstructionKey></d:DataExchangeInstructionKey>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://ldai1uyt.wdf.sap.corp:44300/sap/opu/odata/sap/ZMIN_ASSIGNMENT_SUPPLIER_SRV/ZA_SupplierApi('1')</id>
        <title type="text">ZA_SupplierApi('1')</title>
        <updated>2022-06-29T18:00:32Z</updated>
        <category term="ZMIN_ASSIGNMENT_SUPPLIER_SRV.ZA_SupplierApiType" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme"/>
        <link href="ZA_SupplierApi('1')" rel="self" title="ZA_SupplierApiType"/>
        <link href="ZA_SupplierApi('1')/to_SupplierCompany" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierCompany" type="application/atom+xml;type=feed" title="to_SupplierCompany"/>
        <link href="ZA_SupplierApi('1')/to_SupplierPurchasingOrg" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierPurchasingOrg" type="application/atom+xml;type=feed" title="to_SupplierPurchasingOrg"/>
        <link href="ZA_SupplierApi('1')/to_SupplierText" rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/to_SupplierText" type="application/atom+xml;type=feed" title="to_SupplierText"/>
        <content type="application/xml">
            <m:properties xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
                xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices">
                <d:Supplier>1</d:Supplier>
                <d:AlternativePayeeAccountNumber></d:AlternativePayeeAccountNumber>
                <d:AuthorizationGroup></d:AuthorizationGroup>
                <d:CreatedByUser>GRIMMCHRI</d:CreatedByUser>
                <d:CreationDate>2018-08-14T00:00:00</d:CreationDate>
                <d:Customer>517736</d:Customer>
                <d:PaymentIsBlockedForSupplier>false</d:PaymentIsBlockedForSupplier>
                <d:PostingIsBlocked>false</d:PostingIsBlocked>
                <d:PurchasingIsBlocked>false</d:PurchasingIsBlocked>
                <d:SupplierAccountGroup>0002</d:SupplierAccountGroup>
                <d:SupplierFullName>Mr. Good Supplier/69190 Walldorf</d:SupplierFullName>
                <d:SupplierName>Good Supplier</d:SupplierName>
                <d:VATRegistration></d:VATRegistration>
                <d:BirthDate m:null="true"/>
                <d:ConcatenatedInternationalLocNo>0000000 &amp;00000 &amp;0</d:ConcatenatedInternationalLocNo>
                <d:DeletionIndicator>false</d:DeletionIndicator>
                <d:FiscalAddress></d:FiscalAddress>
                <d:Industry></d:Industry>
                <d:InternationalLocationNumber1>0000000</d:InternationalLocationNumber1>
                <d:InternationalLocationNumber2>00000</d:InternationalLocationNumber2>
                <d:InternationalLocationNumber3>0</d:InternationalLocationNumber3>
                <d:IsNaturalPerson>X</d:IsNaturalPerson>
                <d:ResponsibleType></d:ResponsibleType>
                <d:SuplrQltyInProcmtCertfnValidTo m:null="true"/>
                <d:SuplrQualityManagementSystem></d:SuplrQualityManagementSystem>
                <d:SupplierCorporateGroup></d:SupplierCorporateGroup>
                <d:SupplierProcurementBlock></d:SupplierProcurementBlock>
                <d:TaxNumber1></d:TaxNumber1>
                <d:TaxNumber2></d:TaxNumber2>
                <d:TaxNumber3></d:TaxNumber3>
                <d:TaxNumber4></d:TaxNumber4>
                <d:TaxNumber5></d:TaxNumber5>
                <d:TaxNumberResponsible></d:TaxNumberResponsible>
                <d:TaxNumberType></d:TaxNumberType>
                <d:SuplrProofOfDelivRlvtCode></d:SuplrProofOfDelivRlvtCode>
                <d:BR_TaxIsSplit>false</d:BR_TaxIsSplit>
                <d:DataExchangeInstructionKey></d:DataExchangeInstructionKey>
            </m:properties>
        </content>
    </entry>
</feed>

```

### 2. Use CDS Annotation @OData.publish

We can automatically create an OData service on top of the CDS view using an annotation `@OData.publish:true`.

![CREATE_WITH_ANNOTATIONS](CDS/Segw%20Project/img/annotation_auto_publish.png)

On Activation of CDS view an OData service will be automatically generated with the name <CDS_View_name>_CDS.

OData service created via this annotations has below limitations.

1. This API will be a read only API and if in future an enhancement to support CUD is required then we cannot enhance it.
2. Multilevel associations/navigation is not supported.

As per the requirement of read only and a simple supplier master data API we can also design our OData service using this approach.

A CDS view [ZA_SupApiCdsAnnotation](../Supplier-Api/CDS/DDL/ZA_SupApiCdsAnnotation.txt) is created with annotation `@OData.publish: true` and the OData service is available as shown below.

![AUTO_PUBLISH](CDS/Segw%20Project/img/automatic_odata_service.png)

![TEST_AUTO_PUBLISH](CDS/Segw%20Project/img/test_auto_publish.png)
