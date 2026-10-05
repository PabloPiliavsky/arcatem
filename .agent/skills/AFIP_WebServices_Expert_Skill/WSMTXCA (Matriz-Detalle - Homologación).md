<wsdl:definitions xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/" xmlns:tns="http://impl.service.wsmtxca.afip.gov.ar/service/" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/" name="service" targetNamespace="http://impl.service.wsmtxca.afip.gov.ar/service/">
<wsdl:types>
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema" targetNamespace="http://impl.service.wsmtxca.afip.gov.ar/service/">
<xsd:element maxOccurs="1" minOccurs="1" name="appserver" type="xsd:string"/>
</xsd:schema>
</wsdl:types>
<wsdl:message name="consultarTiposComprobanteRequest">
<wsdl:part name="parameters" element="tns:consultarTiposComprobanteRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarMonedasRequest">
<wsdl:part name="parameters" element="tns:consultarMonedasRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarTiposDocumentoResponse">
<wsdl:part name="parameters" element="tns:consultarTiposDocumentoResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarComprobanteRequest">
<wsdl:part name="parameters" element="tns:consultarComprobanteRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarPtosVtaCAEANoInformadosRequest">
<wsdl:part name="parameters" element="tns:consultarPtosVtaCAEANoInformadosRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarCondicionesIVAReceptorResponse">
<wsdl:part name="parameters" element="tns:consultarCondicionesIVAReceptorResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarUnidadesMedidaRequest">
<wsdl:part name="parameters" element="tns:consultarUnidadesMedidaRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarPtosVtaCAEANoInformadosResponse">
<wsdl:part name="parameters" element="tns:consultarPtosVtaCAEANoInformadosResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarActividadesVigentesRequest">
<wsdl:part name="parameters" element="tns:consultarActividadesVigentesRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarCondicionesIVARequest">
<wsdl:part name="parameters" element="tns:consultarCondicionesIVARequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="informarAjusteIVACAEARequest">
<wsdl:part name="parameters" element="tns:informarAjusteIVACAEARequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarTiposDatosAdicionalesResponse">
<wsdl:part name="parameters" element="tns:consultarTiposDatosAdicionalesResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarActividadesVigentesResponse">
<wsdl:part name="parameters" element="tns:consultarActividadesVigentesResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="autorizarAjusteIVAResponse">
<wsdl:part name="parameters" element="tns:autorizarAjusteIVAResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="exception_faultMsg">
<wsdl:part name="parameters" element="tns:exceptionResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarAlicuotasIVARequest">
<wsdl:part name="parameters" element="tns:consultarAlicuotasIVARequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarMonedasResponse">
<wsdl:part name="parameters" element="tns:consultarMonedasResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="informarCAEANoUtilizadoPtoVtaResponse">
<wsdl:part name="parameters" element="tns:informarCAEANoUtilizadoPtoVtaResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarPuntosVentaCAEAResponse">
<wsdl:part name="parameters" element="tns:consultarPuntosVentaCAEAResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarUnidadesMedidaResponse">
<wsdl:part name="parameters" element="tns:consultarUnidadesMedidaResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="informarCAEANoUtilizadoResponse">
<wsdl:part name="parameters" element="tns:informarCAEANoUtilizadoResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarUltimoComprobanteAutorizadoResponse">
<wsdl:part name="parameters" element="tns:consultarUltimoComprobanteAutorizadoResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarPuntosVentaCAEResponse">
<wsdl:part name="parameters" element="tns:consultarPuntosVentaCAEResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarTiposDocumentoRequest">
<wsdl:part name="parameters" element="tns:consultarTiposDocumentoRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarPuntosVentaResponse">
<wsdl:part name="parameters" element="tns:consultarPuntosVentaResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="solicitarCAEARequest">
<wsdl:part name="parameters" element="tns:solicitarCAEARequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarCAEARequest">
<wsdl:part name="parameters" element="tns:consultarCAEARequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarTiposTributoResponse">
<wsdl:part name="parameters" element="tns:consultarTiposTributoResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="solicitarCAEAResponse">
<wsdl:part name="parameters" element="tns:solicitarCAEAResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="dummyResponse">
<wsdl:part name="parameters" element="tns:dummyResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarPuntosVentaCAERequest">
<wsdl:part name="parameters" element="tns:consultarPuntosVentaCAERequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarPuntosVentaRequest">
<wsdl:part name="parameters" element="tns:consultarPuntosVentaRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarCotizacionMonedaRequest">
<wsdl:part name="parameters" element="tns:consultarCotizacionMonedaRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarCAEAResponse">
<wsdl:part name="parameters" element="tns:consultarCAEAResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="informarCAEANoUtilizadoPtoVtaRequest">
<wsdl:part name="parameters" element="tns:informarCAEANoUtilizadoPtoVtaRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="autorizarAjusteIVARequest">
<wsdl:part name="parameters" element="tns:autorizarAjusteIVARequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarCotizacionMonedaResponse">
<wsdl:part name="parameters" element="tns:consultarCotizacionMonedaResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarCondicionesIVAReceptorRequest">
<wsdl:part name="parameters" element="tns:consultarCondicionesIVAReceptorRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarComprobanteResponse">
<wsdl:part name="parameters" element="tns:consultarComprobanteResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarTiposTributoRequest">
<wsdl:part name="parameters" element="tns:consultarTiposTributoRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarAlicuotasIVAResponse">
<wsdl:part name="parameters" element="tns:consultarAlicuotasIVAResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="autorizarComprobanteRequest">
<wsdl:part name="parameters" element="tns:autorizarComprobanteRequest">
<wsdl:documentation/>
</wsdl:part>
</wsdl:message>
<wsdl:message name="informarComprobanteCAEAResponse">
<wsdl:part name="parameters" element="tns:informarComprobanteCAEAResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="autorizarComprobanteResponse">
<wsdl:part name="parameters" element="tns:autorizarComprobanteResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarPuntosVentaCAEARequest">
<wsdl:part name="parameters" element="tns:consultarPuntosVentaCAEARequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarCondicionesIVAResponse">
<wsdl:part name="parameters" element="tns:consultarCondicionesIVAResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarTiposDatosAdicionalesRequest">
<wsdl:part name="parameters" element="tns:consultarTiposDatosAdicionalesRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="informarComprobanteCAEARequest">
<wsdl:part name="parameters" element="tns:informarComprobanteCAEARequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarCAEAEntreFechasRequest">
<wsdl:part name="parameters" element="tns:consultarCAEAEntreFechasRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="dummyRequest"> </wsdl:message>
<wsdl:message name="consultarUltimoComprobanteAutorizadoRequest">
<wsdl:part name="parameters" element="tns:consultarUltimoComprobanteAutorizadoRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="informarCAEANoUtilizadoRequest">
<wsdl:part name="parameters" element="tns:informarCAEANoUtilizadoRequest"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarTiposComprobanteResponse">
<wsdl:part name="parameters" element="tns:consultarTiposComprobanteResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="consultarCAEAEntreFechasResponse">
<wsdl:part name="parameters" element="tns:consultarCAEAEntreFechasResponse"> </wsdl:part>
</wsdl:message>
<wsdl:message name="informarAjusteIVACAEAResponse">
<wsdl:part name="parameters" element="tns:informarAjusteIVACAEAResponse"> </wsdl:part>
</wsdl:message>
<wsdl:portType name="MTXCAServicePortType">
<wsdl:operation name="dummy">
<wsdl:input message="tns:dummyRequest"> </wsdl:input>
<wsdl:output message="tns:dummyResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="autorizarComprobante">
<wsdl:documentation/>
<wsdl:input message="tns:autorizarComprobanteRequest"> </wsdl:input>
<wsdl:output message="tns:autorizarComprobanteResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="solicitarCAEA">
<wsdl:input message="tns:solicitarCAEARequest"> </wsdl:input>
<wsdl:output message="tns:solicitarCAEAResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="informarComprobanteCAEA">
<wsdl:input message="tns:informarComprobanteCAEARequest"> </wsdl:input>
<wsdl:output message="tns:informarComprobanteCAEAResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarUltimoComprobanteAutorizado">
<wsdl:input message="tns:consultarUltimoComprobanteAutorizadoRequest"> </wsdl:input>
<wsdl:output message="tns:consultarUltimoComprobanteAutorizadoResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarComprobante">
<wsdl:input message="tns:consultarComprobanteRequest"> </wsdl:input>
<wsdl:output message="tns:consultarComprobanteResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarTiposComprobante">
<wsdl:input message="tns:consultarTiposComprobanteRequest"> </wsdl:input>
<wsdl:output message="tns:consultarTiposComprobanteResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarTiposDocumento">
<wsdl:input message="tns:consultarTiposDocumentoRequest"> </wsdl:input>
<wsdl:output message="tns:consultarTiposDocumentoResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarAlicuotasIVA">
<wsdl:input message="tns:consultarAlicuotasIVARequest"> </wsdl:input>
<wsdl:output message="tns:consultarAlicuotasIVAResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarCondicionesIVA">
<wsdl:input message="tns:consultarCondicionesIVARequest"> </wsdl:input>
<wsdl:output message="tns:consultarCondicionesIVAResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarMonedas">
<wsdl:input message="tns:consultarMonedasRequest"> </wsdl:input>
<wsdl:output message="tns:consultarMonedasResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarCotizacionMoneda">
<wsdl:input message="tns:consultarCotizacionMonedaRequest"> </wsdl:input>
<wsdl:output message="tns:consultarCotizacionMonedaResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarUnidadesMedida">
<wsdl:input message="tns:consultarUnidadesMedidaRequest"> </wsdl:input>
<wsdl:output message="tns:consultarUnidadesMedidaResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarTiposTributo">
<wsdl:input message="tns:consultarTiposTributoRequest"> </wsdl:input>
<wsdl:output message="tns:consultarTiposTributoResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarPuntosVenta">
<wsdl:input message="tns:consultarPuntosVentaRequest"> </wsdl:input>
<wsdl:output message="tns:consultarPuntosVentaResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarPuntosVentaCAE">
<wsdl:input message="tns:consultarPuntosVentaCAERequest"> </wsdl:input>
<wsdl:output message="tns:consultarPuntosVentaCAEResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarPuntosVentaCAEA">
<wsdl:input message="tns:consultarPuntosVentaCAEARequest"> </wsdl:input>
<wsdl:output message="tns:consultarPuntosVentaCAEAResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="informarCAEANoUtilizado">
<wsdl:input message="tns:informarCAEANoUtilizadoRequest"> </wsdl:input>
<wsdl:output message="tns:informarCAEANoUtilizadoResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="informarCAEANoUtilizadoPtoVta">
<wsdl:input message="tns:informarCAEANoUtilizadoPtoVtaRequest"> </wsdl:input>
<wsdl:output message="tns:informarCAEANoUtilizadoPtoVtaResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarPtosVtaCAEANoInformados">
<wsdl:input message="tns:consultarPtosVtaCAEANoInformadosRequest"> </wsdl:input>
<wsdl:output message="tns:consultarPtosVtaCAEANoInformadosResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarCAEA">
<wsdl:input message="tns:consultarCAEARequest"> </wsdl:input>
<wsdl:output message="tns:consultarCAEAResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarCAEAEntreFechas">
<wsdl:input message="tns:consultarCAEAEntreFechasRequest"> </wsdl:input>
<wsdl:output message="tns:consultarCAEAEntreFechasResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="autorizarAjusteIVA">
<wsdl:input message="tns:autorizarAjusteIVARequest"> </wsdl:input>
<wsdl:output message="tns:autorizarAjusteIVAResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="informarAjusteIVACAEA">
<wsdl:input message="tns:informarAjusteIVACAEARequest"> </wsdl:input>
<wsdl:output message="tns:informarAjusteIVACAEAResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarTiposDatosAdicionales">
<wsdl:input message="tns:consultarTiposDatosAdicionalesRequest"> </wsdl:input>
<wsdl:output message="tns:consultarTiposDatosAdicionalesResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarActividadesVigentes">
<wsdl:input message="tns:consultarActividadesVigentesRequest"> </wsdl:input>
<wsdl:output message="tns:consultarActividadesVigentesResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarCondicionesIVAReceptor">
<wsdl:input message="tns:consultarCondicionesIVAReceptorRequest"> </wsdl:input>
<wsdl:output message="tns:consultarCondicionesIVAReceptorResponse"> </wsdl:output>
<wsdl:fault name="exception" message="tns:exception_faultMsg"> </wsdl:fault>
</wsdl:operation>
</wsdl:portType>
<wsdl:binding name="MTXCAServiceSoap11Binding" type="tns:MTXCAServicePortType">
<soap:binding style="document" transport="http://schemas.xmlsoap.org/soap/http"/>
<wsdl:operation name="dummy">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/dummy"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="autorizarComprobante">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/autorizarComprobante"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="solicitarCAEA">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/solicitarCAEA"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="informarComprobanteCAEA">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/informarComprobanteCAEA"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarUltimoComprobanteAutorizado">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarUltimoComprobanteAutorizado"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarComprobante">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarComprobante"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarTiposComprobante">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarTiposComprobante"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarTiposDocumento">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarTiposDocumento"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarAlicuotasIVA">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarAlicuotasIVA"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarCondicionesIVA">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarCondicionesIVA"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarCondicionesIVAReceptor">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarCondicionesIVAReceptor"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarMonedas">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarMonedas"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarCotizacionMoneda">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarCotizacionMoneda"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarUnidadesMedida">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarUnidadesMedida"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarPuntosVenta">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarPuntosVenta"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarPuntosVentaCAE">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarPuntosVentaCAE"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarPuntosVentaCAEA">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarPuntosVentaCAEA"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="informarCAEANoUtilizado">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/informarCAEANoUtilizado"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarTiposTributo">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarTiposTributo"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="informarCAEANoUtilizadoPtoVta">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/informarCAEANoUtilizadoPtoVta"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarCAEA">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarCAEA"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarPtosVtaCAEANoInformados">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarPtosVtaCAEANoInformados"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarCAEAEntreFechas">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarCAEAEntreFechas"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="autorizarAjusteIVA">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/autorizarAjusteIVA"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="informarAjusteIVACAEA">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/informarAjusteIVACAEA"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarTiposDatosAdicionales">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarTiposDatosAdicionales"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
<wsdl:operation name="consultarActividadesVigentes">
<soap:operation soapAction="http://impl.service.wsmtxca.afip.gov.ar/service/consultarActividadesVigentes"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
<wsdl:fault name="exception">
<soap:fault name="exception" use="literal"/>
</wsdl:fault>
</wsdl:operation>
</wsdl:binding>
<wsdl:service name="MTXCAService">
<wsdl:port name="MTXCAServiceHttpSoap11Endpoint" binding="tns:MTXCAServiceSoap11Binding">
<soap:address location="https://fwshomo.afip.gov.ar/wsmtxca/services/MTXCAService"/>
</wsdl:port>
</wsdl:service>
</wsdl:definitions>
