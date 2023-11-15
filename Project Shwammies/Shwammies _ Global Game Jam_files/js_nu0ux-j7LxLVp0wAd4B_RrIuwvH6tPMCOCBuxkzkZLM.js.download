(function ($) {

  $(document).ready(function() {
    globalgamejam_themeStickyFooter();
  });

  /**
   * The recommended way for producing HTML markup through JavaScript is to write
   * theming functions. These are similiar to the theming functions that you might
   * know from 'phptemplate' (the default PHP templating engine used by most
   * Drupal themes including Omega). JavaScript theme functions accept arguments
   * and can be overriden by sub-themes.
   *
   * In most cases, there is no good reason to NOT wrap your markup producing
   * JavaScript in a theme function.
   */
  // Drupal.theme.prototype.globalgamejam_themeExampleButton = function (path, title) {
  //   // Create an anchor element with jQuery.
  //   return $('<a href="' + path + '" title="' + title + '">' + title + '</a>');
  // };

  /**
   * Theme game listing exposed filter fieldset for mobile.
   */
  Drupal.theme.prototype.globalgamejam_themeGameListingFilters = function() {
    var exposed_form;
    if ($('.views-exposed-form-games-listing-page').length) {
      exposed_form = $('.views-exposed-form-games-listing-page');
    }
    else {
      exposed_form = $('.views-exposed-form-games-solr-page');
    }

    if (exposed_form.length) {
      exposed_form.addClass('not-collapsible-filter');

      $('<fieldset></fieldset>')
        .addClass('collapsible form-wrapper collapsed collapsible-filter')
        .appendTo('.view-filters')
        .append('<legend><span class="fieldset-legend"><a class="fieldset-title" href="#"><span class="fieldset-legend-prefix element-invisible">Hide</span> Filter</a><span class="summary"></span></span></legend>')
        .append(
          $('<div class="fieldset-wrapper" style="overflow: hidden;"></div>')
            .append(
              exposed_form
                .clone()
                .removeClass('not-collapsible-filter')
            )
        );
    }
    else {
      console.log('Could not find exposed form');
    }
  };

  /**
   * Behaviors are Drupal's way of applying JavaScript to a page. The advantage
   * of behaviors over simIn short, the advantage of Behaviors over a simple
   * document.ready() lies in how it interacts with content loaded through Ajax.
   * Opposed to the 'document.ready()' event which is only fired once when the
   * page is initially loaded, behaviors get re-executed whenever something is
   * added to the page through Ajax.
   *
   * You can attach as many behaviors as you wish. In fact, instead of overloading
   * a single behavior with multiple, completely unrelated tasks you should create
   * a separate behavior for every separate task.
   *
   * In most cases, there is no good reason to NOT wrap your JavaScript code in a
   * behavior.
   *
   * @param context
   *   The context for which the behavior is being executed. This is either the
   *   full page or a piece of HTML that was just added through Ajax.
   * @param settings
   *   An array of settings (added through drupal_add_js()). Instead of accessing
   *   Drupal.settings directly you should use this because of potential
   *   modifications made by the Ajax callback that also produced 'context'.
   */
  Drupal.behaviors.globalgamejam_themeLoginShowLink = {
    attach: function (context, settings) {
      // By using the 'context' variable we make sure that our code only runs on
      // the relevant HTML. Furthermore, by using jQuery.once() we make sure that
      // we don't run the same piece of code for an HTML snippet that we already
      // processed previously. By using .once('foo') all processed elements will
      // get tagged with a 'foo-processed' class, causing all future invocations
      // of this behavior to ignore them.
      $('.l-user-links h2.block__title', context).once('user-links-show', function () {
        var loginText = $(this).text()
        var link = $('<a></a>').attr('href', '/user').text(loginText).click(function(event) {
          event.preventDefault();

          $('.l-user-links .block .block__content').slideToggle();
        });

        $(this).html(link);
      });

      if ($('.l-user-links .error').length > 0 && $('.l-user-links .block .block__content').not(':visible')) {
        $('.l-user-links .block .block__content').slideDown();
      }
    }
  };

  /**
   * Extend the length of the main content on pages with short content.
   */
  Drupal.behaviors.globalgamejam_themeStickyFooterBreakpoint = {
    attach: function (context, settings) {
      $('body').bind('responsivelayout', function(evt, newBreakpoint) {
        Drupal.behaviors.globalgamejam_themeStickyFooter.attach(context, settings);
      });
    }
  };

  Drupal.behaviors.globalgamejam_themeStickyFooter = {
    attach: function (context, settings) {
      globalgamejam_themeStickyFooter();

      // If the
      var omegaSettings = settings.omega || {};
      var mediaQueries = omegaSettings.mediaQueries || {};

      var currentBreakpoint;
      $.each(mediaQueries, function (index, value) {
        var mql = window.matchMedia(value);

        if (mql.matches == true) {
          currentBreakpoint = index;
        }
      });

      switch (currentBreakpoint) {
        case 'wide':
        case 'normal':
        case 'narrow':
          // Don't trigger sticky footer on ever window resize
          // Drupal.theme('globalgamejam_themeStickyFooter');
          $(window).off('resize', globalgamejam_themeStickyFooter);
          break;
        case 'mobile':
          // Trigger sticky footer on every window resize
          $(window).on('resize', globalgamejam_themeStickyFooter);
          break;
      }
    }
  };

  /**
   * Override Omega's Media Query Class handler.
   */
  Drupal.behaviors.omegaMediaQueryClasses.handler = function (name, mql) {
    $.event.trigger('responsivelayout', {current: name});

    if (mql.matches) {
      $('body').removeClass(name + '-inactive').addClass(name + '-active');
    }
    else {
      $('body').removeClass(name + '-active').addClass(name + '-inactive');
    }
  }

  /**
   * Highlight filled in input fields.
   */
  Drupal.behaviors.highlightFormFields = {
    attach: function(context, settings) {
      $('input').bind('blur', function(evt) {
        if ($(this).val().length > 0) {
          $(this).addClass('filled-field');
        }
        else {
          $(this).removeClass('filled-field');
        }
      });
      $('input[type="checkbox"]').bind('change', function(evt) {
        if ($(this).is(':checked')) {
          $('label[for="' + $(this).attr('id') + '"]').addClass('filled-field');
        }
        else {
          $('label[for="' + $(this).attr('id') + '"]').removeClass('filled-field');
        }
      });
    }
  }

  /**
   * Re-arrange password fields.
   */
  Drupal.behaviors.rearrangePasswordFields = {
    attach: function(context, settings) {
      $('.form-item-pass-pass2 > div.password-confirm').insertAfter('input.password-confirm').addClass('password-match');
      $('.form-item-pass-pass1 > div.password-strength').insertAfter('input.password-field');
    }
  }

  /**
   * Sponsors carousel
   */
  Drupal.behaviors.globalgamejam_sponsorCarouselBreakpoint = {
    attach: function (context, settings) {
      $('body').bind('responsivelayout', function(evt, newBreakpoint) {
        Drupal.behaviors.globalgamejam_sponsorCarousel.attach(context, settings);
      });
    }
  };


  Drupal.behaviors.globalgamejam_sponsorCarousel = {
    attach: function (context, settings) {
      var omegaSettings = settings.omega || {};
      var mediaQueries = omegaSettings.mediaQueries || {};

      var currentBreakpoint;
      $.each(mediaQueries, function (index, value) {
        var mql = window.matchMedia(value);

        if (mql.matches == true) {
          currentBreakpoint = index;
        }
      });

      var container = $('.block--views-sponsors-additional-block .view-display-id-additional_block .view-content > .item-list');
      var contents = container.html();
      var selector = 'ul > li.views-row',
          animation = 'slide',
          slideshowSpeed = 5000,
          reverse = false,
          pauseOnHover = true,
          setFooter = function(){
            globalgamejam_themeStickyFooter();
          };


      switch (currentBreakpoint) {
        case 'wide':
          $('.block--views-sponsors-additional-block .view-display-id-additional_block .view-content', context).children('.item-list').remove().end().prepend('<div class="item-list"></div>').children('.item-list').html(contents).flexslider({
            itemWidth: 164,
            itemMargin: 27,
            selector: selector,
            animation: animation,
            slideshowSpeed: slideshowSpeed,
            reverse: reverse,
            pauseOnHover: pauseOnHover,
            start: setFooter
          });
        break;
        case 'normal':
          $('.block--views-sponsors-additional-block .view-display-id-additional_block .view-content', context).children('.item-list').remove().end().prepend('<div class="item-list"></div>').children('.item-list').html(contents).flexslider({
            itemWidth: 128,
            itemMargin: 21,
            selector: selector,
            animation: animation,
            slideshowSpeed: slideshowSpeed,
            reverse: reverse,
            pauseOnHover: pauseOnHover,
            start: setFooter
          });
        break;
        case 'narrow':
          $('.block--views-sponsors-additional-block .view-display-id-additional_block .view-content', context).children('.item-list').remove().end().prepend('<div class="item-list"></div>').children('.item-list').html(contents).flexslider({
            itemWidth: 152,
            itemMargin: 25,
            selector: selector,
            animation: animation,
            slideshowSpeed: slideshowSpeed,
            reverse: reverse,
            pauseOnHover: pauseOnHover,
            start: setFooter
          });
        break;
        case 'mobile':
          $('.block--views-sponsors-additional-block .view-display-id-additional_block .view-content', context).children('.item-list').remove().end().prepend('<div class="item-list"></div>').children('.item-list').html(contents).flexslider({
            itemWidth: 129,
            itemMargin: 21,
            selector: selector,
            animation: animation,
            slideshowSpeed: slideshowSpeed,
            reverse: reverse,
            pauseOnHover: pauseOnHover,
            start: setFooter,
            before: setFooter
          });
        break;

      }
    }
  };

  var globalgamejam_themeStickyFooter = function() {
    // // Unset the height of the content.
    $('.l-content').height('inherit');

    var winHeight = $(window).height();
    var footerHeight = $('.l-footer').height();
    var mainMarginTop = $('.l-main').offset().top;

    // Fix the main section padding bottom to exactly match the footer.
    var paddingBottom = parseInt(footerHeight);
    $('.l-main').css('padding-bottom', paddingBottom);

    // If the height of the content is smaller than the white background,
    // then make the white background the full height.
    var winHeight = $(window).height();
    var footerHeight = $('.l-footer').height();
    var contentTop = $('.l-content').offset().top;
    var whiteContent = winHeight - footerHeight - contentTop - mainMarginTop;
    if ($('.l-content').height() < whiteContent) {
      $('.l-content').height(whiteContent);
    }
  };

  /**
   * Make a collapsible fieldset to appear on the game listings page on mobile.
   */
  Drupal.behaviors.globalgamejam_themeGameListingFilters = {
    attach: function (context, settings) {
      if (
        ($('.view-id-games.view-display-id-listing_page').length > 0 && $('.view-id-games.view-display-id-listing_page fieldset').length < 1) ||
        ($('.view-id-games_solr.view-display-id-page').length > 0 && $('.view-id-games_solr.view-display-id-page fieldset').length < 1)
        ) {
        Drupal.theme('globalgamejam_themeGameListingFilters');
        Drupal.attachBehaviors();
      }
    }
  };

})(jQuery);
;
