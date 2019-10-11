/**
 * All of the code for your public-facing JavaScript source
 * should reside in this file.
 */

(function($) {
    "use strict";
    $(function() {
        /*------------------------------------------------*
         *  Calculating paypal charges on keyup event
         * -----------------------------------------------*/

        // Timer for the timeout
        var amount = '';
        var initial;
        var gateway_fee = '';
        $(document).on('keyup blur change', '#actualamount, #amount', function() {
            var current = $(this).closest('fieldset');
            gateway_fee = $(this).closest('form').find('input[name="gateway"]:checked').attr('data-fee');
            if (amount !== current.find('#actualamount').val()) {
                current.find('.fees').show();
                current.find('#payFees').html('<img class="wppd_form_loader" src="' + wppd_display.loader_url + '" alt="loading..."  />');
                clearTimeout(initial);
                initial = window.setTimeout(function() {

                    var response = calculateAmount(amount, gateway_fee, current);

                    amount = response.new_amount;
                    gateway_fee = response.new_fee;


                }, 1500);
                return false;
            } else {

            }
        });
        
          //====Instanse Payment =========
        
        var CharityCreditCard={
            init: function(button){
                //	alert("ccc");                    
                //alert($(button).parents("form").serialize()+"&action=aaaaaaaaaaaaaaaa");
               this.loader();
                this.sendForm(button);
                
            },
        		
            sendForm: function(button){
                var self=this;
                $.ajax({
                    type: "POST",
                    url: wppd_display.ajaxurl,
                    data: $(button).parents("form").serialize()+"&action=easyCreditSystem",
                    success: function(msg) {
                                         
                        try{
                            var obj=JSON.parse(msg);
                            if(obj.msg == "success"){
                                self.redirect(obj.redirect_url);
                            }
                        }   
                        catch(e){
                            $("body .payment-errors").html(msg);
                            //$("#creditCardError").html(msg);
                        }
                    }
                });
	
            },
            loader: function(){
               // $("#creditCardError").remove();
                //console.log($("#submit-ccard"));
                //$(".donate-form #submit-ccard").after("<div id='creditCardError'></div>");
                $(document).ajaxStart( function(){
                    $("body .payment-errors").html("Loading ....");
                    //$("#creditCardError").html("Loading ....");
                });
                $(document).ajaxStop( function(){
                    
                    });
            },
                        
            redirect: function($location){
                window.location.href=$location;                           
            }                               
        };
              
        //=============================
             
        $(document).on("click", ".wppd-form #payDirectSubmit", function(){       	
          
            var self=this;	
            //var validationSuccess=false;
            var isCreditCard=$(".wppd-form input[name='gateway']:checked").val();
           
            // =============================
            if(isCreditCard == "ccard"){
                $("#submit-stripe").find("input").removeAttr("required");
                $("#submit-stripe").find("select").removeAttr("required");
                $("#submit-ccard").find("input").attr("required", "");
                $("#submit-ccard").find("select").attr("required", "");
            }
            else if(isCreditCard == "stripe"){
                $("#submit-stripe").find("input").attr("required", "");
                $("#submit-stripe").find("select").attr("required", "");
                $("#submit-ccard").find("input").removeAttr("required");
                $("#submit-ccard").find("select").removeAttr("required");
            }
            else{
                $("#submit-stripe").find("input").removeAttr("required");
                $("#submit-stripe").find("select").removeAttr("required");
                $("#submit-ccard").find("input").removeAttr("required");
                $("#submit-ccard").find("select").removeAttr("required");
            }     
            
       
            //	=======================
              
            $('.donate-form .wppd-form').validate({
                highlight: function(element) {
                    //validationSuccess=true;
                    $(element).closest('.form-group').removeClass('has-success').addClass('has-error');
                },
                unhighlight: function(element) {
                    $(element).closest('.form-group').removeClass('has-error').addClass('has-success');
                },
                errorElement: 'span',
                errorClass: 'help-block',
                errorPlacement: function(error, element) {
                    if (element.parent('.input-group').length) {
                        error.insertAfter(element.parent());
                    } else {
                        error.insertAfter(element);
                    }
                },
                rules: {
                    actualamount: {
                        required: true,
                        number: true
                    },
                    email: {
                        required: true,
                        email: true
                    },
                },
                submitHandler: function(form) {
                    //==================
                    if(isCreditCard == "ccard"){
                        CharityCreditCard.init(self); //CreditCard
                        return false;
                    }
                    else if(isCreditCard == "stripe_ccard"){//stripe
                        //jQuery("#stripe-error").remove();
                        //jQuery("#submit-stripe").append("<div id='stripe-error'>Loading ... </div>");
                         $("body .payment-errors").html("Loading ....");
                        // createToken returns immediately - the supplied callback submits the form if there are no errors
                        
                   
                        //return false;
                        
                        Stripe.card.createToken({
                            number: $('#stripe_card_number').val(),
                            cvc: $('#stripe_cvv').val(),
                            name: $('#stripe_card_holder_name').val(),
                            //address_line1: $('#address').val(),
                            exp_month : $('#stripe_expiry_month').val(),
                            exp_year : $('#stripe_expiry_year').val(),
                        }, stripeResponseHandler);
                        return false; // submit from callback
                    }
                    else{
                        form.submit();
                    }
                    return false;
                }
                
            });
        });                         
//------------------      
        
        
        
        

        $('#builder-fields input').each(function() {
            $(this).attr('maxlength', '255');
        });

        $('#builder-fields textarea').each(function() {
            $(this).attr('maxlength', '2000');
        });


        $('form.wppd-form input, #submit-ccard input[name="card_holder_name"]').not('form.wppd-form input[type="radio"], form.wppd-form input[type="checkbox"]').on('keyup change blur', function(e) {
            var arr = [8, 37, 38, 39, 40, 46];
            var key = e.keyCode;
            // Don't validate the input if Left, Up, Right, Down Arrow, Backspace, Delete keys were pressed
            // @TODO Remove undefined key error for radio & checkboxes
            if (!isInArray(key, arr)) {
                $(this).val(remove_special_chars($(this).val()));
            }
        });

        // onload
        var amountentered = 0;
        amountentered = $.trim($('#actualamount').val());
        if (amountentered.length > 0) {
            calculateAmount(amount, gateway_fee, '');
        }

        /**
         * Toggle Payment Gateways
         */

        $(document).on('change', 'input[name="gateway"]', function() {
            var $type = $(this).val();
            $(this).closest('fieldset').find('.gateway-form').hide();
            $(this).closest('fieldset').find('#submit-' + $type).show('slow');

            if ($type != 'paypal' && $type != 'ccard') {
                var response = calculateAmount(amount, '0', $(this).closest('form'));
                $(this).closest('form').find('.fees').hide();
            } else {
                var response = calculateAmount(amount, $(this).data('fee'), $(this).closest('form'));
                $(this).closest('form').find('.fees').show();
            }
            amount = response.new_amount;
            gateway_fee = response.new_fee;

        });

        /**
         * Toggle Recurring Time Period Choices
         */

        $(document).on('change', 'input[name="paymenttype"]', function() {
            var $type = $(this).val();
            if ($type == 'recurring') {
                $(this).closest('fieldset').find('#recurring-time-peroid').show('slow');
            } else {
                $(this).closest('fieldset').find('#recurring-time-peroid').hide('slow');
            }
        });

        $(".gateway-form:not(:eq(0))").hide();
         
		
		
		
        /**
         * Toggle amount based on choices
         */

        $(document).on('change', 'input[name="actualamount_option"]', function() {
            $(this).closest('fieldset').find('#actualamount').val($(this).val());
            $(this).closest('fieldset').find('#actualamount').trigger('change');
        });

        /**
         * Avoid cut, copy, paste on credit card values
         */
        $(document).bind("cut copy paste", '#submi1t-ccard input', function(e) {
            e.preventDefault();
        });
    });
    
    // this identifies your website in the createToken call below
   // Stripe.setPublishableKey(wppd_display.stripe_pk_key);
           
    // Disable stripe getway      
    $(document).on("click", "#paymenttype-recurring, #paymenttype-onetime", function(){               	
        var paymentType=$(".wppd-form input[name='paymenttype']:checked").val();    
                
        if(paymentType == 'recurring'){
            $('#stripe').parents(".radio").hide();
            $("#submit-stripe").hide();
        }
        else{
            $('#stripe').parents(".radio").show();
            $("#submit-stripe").show();
        }                
    });    
    
    
})(jQuery);


/**
* 
* @param {string} text
* @returns {unresolved}
*/

function stripeResponseHandler(status, response) {
    
    
    if (response.error) {
        jQuery("body .payment-errors").html(response.error.message);
        // re-enable the submit button
        //jQuery('#payDirectSubmit').removeAttr("disabled");
        // show hidden div
        //jQuery("#stripe-error").remove();
        //jQuery("#submit-stripe").append("<div id='stripe-error'><div class='alert alert-danger' role='alert'>"+response.error.message+"</div></div>");
    } else {
    	
        jQuery('#loadingTxt').css('visibility', 'visible');
        jQuery.ajax({
            type: "POST",
            url: wppd_display.ajaxurl,
            data:jQuery("form.wppd-form").serialize()+"&action=easyStripPayment&stripeToken="+response['id'],
            success: function(msg) {
               
                try{
                    var obj=JSON.parse(msg);
                    if(obj.msg == "success"){
                        window.location.href=obj.redirect_url;
                    }
                }   
                catch(e){
                     jQuery("body .payment-errors").html(msg);
                    //$("#creditCardError").html(msg);
                } 

            }
        });

    }
}


/**
 * 
 * @param {string} text
 * @returns {unresolved}
 */

function remove_special_chars(text) {
    return text.replace(/[^\w\s.@-_]/gi, '');
}

/*-------------------------------------*
 * Function to calculate paypal charges
 *------------------------------------*/

function calculatePaypalTax(amount, fee) {

    var tax = 0;
    tax = (amount * fee) / 100;
    return tax;
}

/*-------------------------------------------*
 *	Function to round off the float values
 *-------------------------------------------*/


function format_number(amount) {

    var i = parseFloat(amount);
    if (isNaN(i)) {
        i = 0.00;
    }
    var minus = '';
    if (i < 0) {
        minus = '-';
    }
    i = Math.abs(i);
    i = parseInt((i + 0.005) * 100);
    i = i / 100;
    var s = String(i);
    if (s.indexOf('.') < 0) {
        s += '.00';
    }
    if (s.indexOf('.') == (s.length - 2)) {
        s += '0';
    }
    s = minus + s;
    return s;
}

/*----------------------------------------------------*
 * 	Function to display total payable amount
 *----------------------------------------------------*/

function calculateAmount(amount, fee, current) {
    if (current != '') {
        var totalamout = current.find('#actualamount').val();
        current.find("#actualamountInfo").text('');
        var tax = 0;
        totalamout = Number(totalamout);
        tax = calculatePaypalTax(totalamout, fee);
        totalamout = format_number(totalamout + tax);
        tax = format_number(tax);
        current.find('#payFees').html(tax);
        current.find('#feesTotal').html(totalamout);
        current.find('#amount').val(totalamout);
        return {'new_amount': current.find('#actualamount').val(), 'new_fee': fee};
    } else {
        return {'new_amount': '0', 'new_fee': '0'};
    }

}

/*----------------------------------------------------*
 * 	Function to check if a value is exist in an array or not
 *----------------------------------------------------*/
function isInArray(value, array) {
    return array.indexOf(value) > -1;
}
