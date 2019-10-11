/**
 * Public Form validation file
 */

(function($) {
    "use strict";
    $(function() {
        $('.wppd-form').each(function() {
            $(this).validate({
                highlight: function(element) {
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
                    }, phone_no: {
                        required: true,
                        number: true
                    },
                    submitHandler: function(form) {
                        $('.wppd-form').submit();
                    }
                }
            });
        });

    });
})(jQuery);
