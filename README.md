# Validacoes-JS
Minhas funções muito úteis em JavaScript com JQuery para validação de formulário

function validaCampos(seletor) {
	  var preenchidos = true;
	  var camposVazios = [];

	  $(seletor).each(function () {
	    if (this.style.display == "none") {
	      return; // Ignora campos escondidos
	    }

	    var campo = $(this);
	    var valorCampo = campo.val().trim();
	    var campoId = campo.attr("id");
	    var label = $("label[for='" + campoId + "']").text().split(" *")[0].trim();

	    if (!valorCampo) {
	      preenchidos = false;
	      campo.addClass('campo-obrigatorio');
	      campo.css('border-color', 'red');
		  campo.css('box-shadow', 'inset 0 1px 1px #ff000040');

	      if (!camposVazios.includes(label)) {
	        camposVazios.push(label);
	      }

	    
	      campo.on('input',function(e) {
	        if ($(this).val().trim() !== "") {
	        	campo.removeClass('campo-obrigatorio');	
    			campo.css('border-color', '');
    			campo.css('box-shadow', 'inset 0 1px 1px rgba(0,0,0,.075)')

	        }
	      });
	      
	      campo.on('change',function(e) {
		        if ($(this).val().trim() !== "") {
		        	campo.removeClass('campo-obrigatorio');	
	    			campo.css('border-color', '');
	    			campo.css('box-shadow', 'inset 0 1px 1px rgba(0,0,0,.075)')

		        }
		      });
	    } else {
	    	campo.removeClass('campo-obrigatorio');	
			campo.css('border-color', '');
			campo.css('box-shadow', 'inset 0 1px 1px rgba(0,0,0,.075)')
	      var index = camposVazios.indexOf(label);
	      if (index > -1) {
	        camposVazios.splice(index, 1);
	      }
	    }
	  });

	  if (!preenchidos) {
	    var msgErro = "Favor preencher: " + camposVazios.join(", ");
	    exibirMensagem(msgErro, "warning");
	    return false;
	  } else {
	    return true;
	  }
}

function validaRadio(seletor){
	var naoMarcados = [];
    var labelsNaoMarcados = [];
    
    $(seletor).each(function() {
        var nomeRadio = $(this).attr('name');
        var assinalado = $("input[name='" + nomeRadio + "']:checked").length > 0;

        if (!assinalado && !naoMarcados.includes(nomeRadio)) {
            naoMarcados.push(nomeRadio);

            var labelRadio = $("label[for='" + nomeRadio + "']").text().split(" *")[0].trim();
            if (!labelsNaoMarcados.includes(labelRadio)) {
            	labelsNaoMarcados.push(labelRadio);
            } 
            $("input[name='" + nomeRadio + "']")
            .addClass('campo-obrigatorio')
            .css('outline', 'auto 1px red')
            .css('box-shadow', 'inset 0 1px 1px #ff000040');
                
        } else if (assinalado){
        	$("input[name='" + nomeRadio + "']")
        	.removeClass('campo-obrigatorio')
        	.css('outline', '')
        	.css('box-shadow', 'inset 0 1px 1px rgba(0,0,0,.075)');
        }
            
    });
	    
    $(seletor).on('change', function(){
    	var nomeRadio = $(this).attr('name');
    	$("input[name='" + nomeRadio + "']")
    	.removeClass('campo-obrigatorio')
    	.css('outline', '')
    	.css('box-shadow', 'inset 0 1px 1px rgba(0,0,0,.075)');
    });
    
    if (labelsNaoMarcados.length > 0){
	    var msgErroRadio = "Favor fornecer uma resposta para: " + labelsNaoMarcados.join(", ");
	    exibirMensagem(msgErroRadio, "warning");
	    return false;
	} return true;	
}

function validaCheck(seletor) {
    var naoMarcados = [];
    var labelsNaoMarcados = [];
    var gruposMarcados = {};
    
    
    $(seletor).each(function() {
        var dataGroup = $(this).attr('data-group');
        if (!gruposMarcados[dataGroup]) {
            gruposMarcados[dataGroup] = false;
        }
    });

    
    $(seletor).each(function() {
        var nomeCheck = $(this).attr('name');
        var assinalado = $("input[name='" + nomeCheck + "']:checked").length > 0;
        var dataGroup = $(this).attr('data-group');

        if (assinalado) {
            gruposMarcados[dataGroup] = true;
        }
    });

    
    $(seletor).each(function() {
        var dataGroup = $(this).attr('data-group');
        var nomeCheck = $(this).attr('name');

        if (!gruposMarcados[dataGroup] && !naoMarcados.includes(dataGroup)) {
            naoMarcados.push(dataGroup);
            var labelCheck = $("label[for='" + dataGroup + "']").text().split(" *")[0].trim();

            if (!labelsNaoMarcados.includes(labelCheck)) {
                labelsNaoMarcados.push(labelCheck);
            }
            $("input[data-group='" + dataGroup + "']").addClass('campo-obrigatorio').css({
                "border": "0.15em solid red",
                "box-shadow": "inset 0 1px 1px #ff000040"
            });
        } else if (gruposMarcados[dataGroup]) {
            $("input[data-group='" + dataGroup + "']").removeClass('campo-obrigatorio').css({
                'border': '0.15em solid #58595b',
                'box-shadow': 'inset 0 1px 1px rgba(0,0,0,.075)'
            });
        }
    });

    $(seletor).on('change', function() {
        var dataGroup = $(this).attr('data-group');
        if ($(this).is(":checked")) {
            $("input[data-group='" + dataGroup + "']").removeClass('campo-obrigatorio').css({
                'border': '0.15em solid #58595b',
                'box-shadow': 'inset 0 1px 1px rgba(0,0,0,.075)'
            });
        }
    });

    if (labelsNaoMarcados.length > 0) {
        var msgErroCheck = "Favor fornecer uma resposta para: " + labelsNaoMarcados.join(", ");
        exibirMensagem(msgErroCheck, "warning");
        return false;
    }
    return true;
}

function validaTextArea(seletor){
	var preenchidos = true;
	var camposVazios = [];
	
	$(seletor).each(function(){
		if (this.style.display== 'none') {
			;
		} else {
			var campo = $(this);
			var valorCampo = campo.val().trim();
			var campoId = campo.attr('id');
			var label = $("label[for='" + campoId + "']").text().split(" *")[0].trim();
			
			if(!valorCampo) {
				preenchidos = false;
				campo.addClass('campo-obrigatorio');
				campo.css('border-color', 'red');
				campo.css('box-shadow', 'inset 0 1px 1px #ff000040');
    			if (!camposVazios.includes(label)) {
    				camposVazios.push(label);
    			}
    			campo.on('input', function(e){
    				campo.removeClass('campo-obrigatorio');	
    				campo.css('border-color', '');
    				campo.css('box-shadow', 'inset 0 1px 1px rgba(0,0,0,.075)')
    			});
			} else {
				campo.removeClass('campo-obrigatorio');	
				campo.css('border-color', '');
				campo.css('box-shadow', 'inset 0 1px 1px rgba(0,0,0,.075)')
    			var index = camposVazios.indexOf(label);
    			if (index > -1) {
    				camposVazios.splice(index, 1);
    			}
			}
		}
	});
	if (!preenchidos) {
        var msgErro = "Favor preencher: " + camposVazios.join(", ");
        exibirMensagem(msgErro, "warning");
        return false;
    } else {
        return true;
    }
}
