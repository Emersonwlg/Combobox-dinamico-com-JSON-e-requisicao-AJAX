# Combobox dinâmico com JSON e requisição AJAX

### JavaScript 

    $selectCliente.on('change', function () {
        $.ajax({
            url: '/Atendimento/BuscarDadosBancariosCliente',
            type: 'GET',
            data: {
                codCliente: $selectCliente.val()
            },
            success: function (data) {
                    $codigoBanco.val(data.CodigoBanco);
                    $agenciaBanco.val(data.AgenciaBanco);
                    $contaBanco.val(data.ContaBanco);
                    $descricaoBanco.val(data.DescricaoBanco);
            }
        });
    });  
    
### Controller  
    
    #region [PROPERTIES]
    
    private readonly IUsuarioRegionalServico _usuarioRegionalServico;
    
    #endregion
    
    #region [CONSTRUCTOR]
    
     public AtendimentoController(IAtendimentoService atendimentoService)
     {
        _atendimentoService = atendimentoService;
     }
    
    #endregion
      
    #region [ACTIONS]
    
    public JsonResult BuscarDadosBancariosCliente(int codCliente)  
    {
        var resultado = PreencherDadosBancarios(codCliente);
        return Json(resultado, JsonRequestBehavior.AllowGet);
    }
    
    #endregion
    
    #region [METHODS]
    
    private CadastroSolicitacaoPagamentoAporteDividendosViewModel PreencherDadosBancarios(int codCliente)
        {
            var model = new CadastroSolicitacaoPagamentoAporteDividendosViewModel();
            var resultado = _atendimentoService.BuscarDadosBancariosCliente(codCliente);

            if (resultado != null)
            {
                model = new CadastroSolicitacaoPagamentoAporteDividendosViewModel()
                {
                    CodigoBanco = resultado.NumBanco,
                    AgenciaBanco = resultado.AgenciaBanco,
                    ContaBanco = resultado.ContaBanco,                    
                    DescricaoBanco = resultado.NomeBanco
                };
            }
            return model;
        }
        
    #endregion
