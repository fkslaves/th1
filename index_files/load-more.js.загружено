(function($) {
	'use strict';

	window.bddexLoadMore = function( options, settings ) {

		// Default Values for Load More Js
		var optionsValue = {
			siteUrl: options.siteUrl,
			totalPosts: options.totalPosts,
			loadMoreBtn: options.loadMoreBtn,
			postContainer: options.postContainer,
			postStyle: options.postStyle, // block, grid, timeline
		}
		// Settings Values
		var settingsValue = {
			postType: settings.postType,
			perPage: settings.perPage,
			postOrder: settings.postOrder,
			showImage: settings.showImage,
			showTitle: settings.showTitle,
			showExcerpt: settings.showExcerpt,
			showMeta: settings.showMeta,
			metaPosition: settings.metaPosition,
			excerptLength: settings.excerptLength,
			btnText: settings.btnText,
			categories: settings.categories,
		}

		var offset = settingsValue.perPage;

		optionsValue.loadMoreBtn.on( 'click', function( e ) {
			e.preventDefault();
			$(this).addClass( 'button--loading' );
			$(this).find( 'span' ).html( 'Loading...' );

			// Rest Api Url Settings
			if( settingsValue.postType == 'post' || settingsValue.postType == 'page' ) {
				if( settings.categories == '' ) {
					var restUrl = optionsValue.siteUrl+'wp-json/wp/v2/'+settings.postType+'s?per_page='+settingsValue.perPage+'&offset='+offset+'&order='+settingsValue.postOrder+'&_embed';
				}else {
					var restUrl = optionsValue.siteUrl+'wp-json/wp/v2/'+settings.postType+'s?categories='+settingsValue.categories+'&per_page='+settingsValue.perPage+'&offset='+offset+'&order='+settingsValue.postOrder+'&_embed';
				}
			}else {
				if( settings.categories == '' ) {
					var restUrl = optionsValue.siteUrl+'wp-json/wp/v2/'+settings.postType+'?per_page='+settingsValue.perPage+'&offset='+offset+'&order='+settingsValue.postOrder+'&_embed';
				}else {
					var restUrl = optionsValue.siteUrl+'wp-json/wp/v2/'+settings.postType+'?categories='+settingsValue.categories+'&per_page='+settingsValue.perPage+'&offset='+offset+'&order='+settingsValue.postOrder+'&_embed';
				}
			}

			$.ajax({
				url: restUrl,
				type: 'GET',
				success: function( res ) {
					createPostHtml( res );
					if( optionsValue.postStyle === 'grid' ) {
						$( '.bddex-post-grid' ).masonry( 'destroy' );
						setTimeout(function() {
							$('.bddex-post-grid').masonry({
						    	itemSelector: '.bddex-grid-post',
						    	percentPosition: true,
						    	columnWidth: '.bddex-post-grid-column'
						    });
						}, 100);

					}
					optionsValue.loadMoreBtn.removeClass( 'button--loading' );
					optionsValue.loadMoreBtn.find( 'span' ).html( settingsValue.btnText );

					offset = offset + settingsValue.perPage;
					if( offset >= optionsValue.totalPosts ) {
						optionsValue.loadMoreBtn.remove();
					}
				},
				error: function( err ) {
					console.log( 'Something went wrong!' );
				}
			});
		} );

		/**
		 * Create Html Post Block
		 */
		function createPostHtml( data ) {

			if( optionsValue.postStyle === 'timeline' ) {
				var html = '';
				for (var i = 0; i < data.length; i++) {
				    // Get Image
				    if (data[i]._links['wp:featuredmedia']) {
				        var feature_image = 'style="background-image: url(' + data[i]._embedded['wp:featuredmedia'][0].source_url + ');"';
				    } else {
				        var feature_image = '';
				    }
				    // Get Date
				    var getPostDate = new Date(data[i].date);

				    html += '<article class="bddex-timeline-post bddex-timeline-column">';
				    html += '<div class="bddex-timeline-bullet"></div>';
				    html += '<div class="bddex-timeline-post-inner">';
				    html += '<a class="bddex-timeline-post-link" href="' + data[i].link + '" title="' + data[i].title.rendered + '">';
				    html += '<time datetime="' + get_post_date(getPostDate) + '">' + get_post_date(getPostDate) + '</time>';
				    html += '<div class="bddex-timeline-post-image" ' + feature_image + ' ></div>';
				    if ( settingsValue.showExcerpt == 1 ) {
				        html += '<div class="bddex-timeline-post-excerpt">';
				        html += '' + data[i].excerpt.rendered.split(/\s+/).slice(0, settingsValue.excerptLength).join(" ");
				        html += '</div>';
				    }
				    if ( settingsValue.showTitle == 1 ) {
				        html += '<div class="bddex-timeline-post-title">';
				        html += '<h2>' + data[i].title.rendered + '</h2>';
				        html += '</div>';
				    }
				    html += '</a>';
				    html += '</div>';
				    html += '</div>';
				    html += '</article>';
				}
				optionsValue.postContainer.append(html);
			}else if( optionsValue.postStyle === 'grid' ) {
				var html = '';
				for( var i = 0; i < data.length; i++ ) {
					// Get Image
					if( data[i]._links['wp:featuredmedia'] ) {
						var feature_image = '<img src="'+data[i]._embedded['wp:featuredmedia'][0].source_url+'" />';
					}else {
						var feature_image = '';
					}
					// Get Date
					var getPostDate = new Date( data[i].date );

					html += '<article class="bddex-grid-post bddex-post-grid-column">';
					html += '<div class="bddex-grid-post-holder">';
					html += '<div class="bddex-grid-post-holder-inner">';
					if( data[i]._links['wp:featuredmedia'] ) {
						html += '<div class="bddex-entry-media">';
						html += '<div class="bddex-entry-overlay">';
						html += '<a href="'+data[i].link+'"></a>';
						html += '</div>';
						if( settingsValue.showImage == 1 ) {
							html += '<div class="bddex-entry-thumbnail">'+feature_image;
							html += '</div>';
						}
						html += '</div>';
					}
					html += '<div class="bddex-entry-wrapper">';

					html += '<header class="bddex-entry-header">';
					if( settingsValue.showTitle == 1 ) {
						html += '<h2 class="bddex-entry-title"><a class="bddex-grid-post-link" href="'+data[i].link+'" title="'+data[i].title.rendered+'">'+data[i].title.rendered+'</a></h2>';
					}
					if( settingsValue.showMeta == 1 && settingsValue.metaPosition == 'meta-entry-header') {
						html += '<div class="bddex-entry-meta">';
						html += '<span class="bddex-posted-by"><a href="'+data[i]._embedded.author[0].link+'">'+data[i]._embedded.author[0].name+'</a></span>';
						html += '<span class="bddex-posted-on"><time datetime="'+get_post_date( getPostDate )+'">'+get_post_date( getPostDate )+'</time></span>';
						html += '</div>';
					}
					html += '</header>';

					html += '<div class="bddex-entry-content">';
					if( settingsValue.showExcerpt == 1 ) {
						html += '<div class="bddex-grid-post-excerpt">';
						html += '<p>'+data[i].excerpt.rendered.split( /\s+/ ).slice( 0, settingsValue.excerptLength ).join( " " )+'...</p>';
						html += '</div>';
					}
					html += '</div>';
					html += '</div>';
					if( settingsValue.showMeta == 1 && settingsValue.metaPosition == 'meta-entry-footer' ) {
						html += '<div class="bddex-entry-footer">';
						html += '<div class="bddex-author-avatar">';
						html += '<a href="'+data[i]._embedded.author[0].link+'">';
						html += '<img src="'+data[i]._embedded.author[0].avatar_urls[96]+'" class="avatar avatar-96 photo" />';
						html += '</a>';
						html += '</div>';
						html += '<div class="bddex-entry-meta">';
						html += '<div class="bddex-posted-by">';
						html += '<a href="'+data[i]._embedded.author[0].link+'">'+data[i]._embedded.author[0].name+'</a>';
						html += '</div>';
						html += '<div class="bddex-posted-on">';
						html += '<time datetime="'+get_post_date( getPostDate )+'">'+get_post_date( getPostDate );
						html += '</time>';
						html += '</div>';
						html += '</div>';
						html += '</div>';
					}
					html += '</div>';
					html += '</article>';
				}
				optionsValue.postContainer.append( html );
			}else if( optionsValue.postStyle === 'block' ) {
				var html = '';
				for ( var i = 0; i < data.length; i++ ) {
				    // Get Image
				    if ( data[i]._links['wp:featuredmedia'] ) {
				        var feature_image = '<img src="' + data[i]._embedded['wp:featuredmedia'][0].source_url + '" />';
				    } else {
				        var feature_image = '';
				    }
				    // Get Date
				    var getPostDate = new Date( data[i].date );

				    html += '<article class="bddex-post-block-item bddex-post-block-column">';
				    html += '<div class="bddex-post-block-item-holder">';
				    html += '<div class="bddex-post-block-item-holder-inner">';
				    if ( settingsValue.showImage == 1 ) {
				        html += '<div class="bddex-entry-media">';
				        html += '<div class="bddex-entry-overlay">';
				        html += '<a href="' + data[i].link + '"></a>';
				        html += '</div>';
				        html += '<div class="bddex-entry-thumbnail">' + feature_image;
				        html += '</div>';
				        html += '</div>';
				    }
				    html += '<div class="bddex-entry-wrapper">';

				    html += '<header class="bddex-entry-header">';
				    if ( settingsValue.showTitle == 1 ) {
				        html += '<h2 class="bddex-entry-title"><a class="bddex-grid-post-link" href="' + data[i].link + '" title="' + data[i].title.rendered + '">' + data[i].title.rendered + '</a></h2>';
				    }
				    if ( settingsValue.showMeta == 1 && settingsValue.metaPosition == 'meta-entry-header' ) {
				        html += '<div class="bddex-entry-meta">';
				        html += '<span class="bddex-posted-by"><a href="' + data[i]._embedded.author[0].link + '">' + data[i]._embedded.author[0].name + '</a></span>';
				        html += '<span class="bddex-posted-on"><time datetime="' + get_post_date( getPostDate ) + '">' + get_post_date( getPostDate ) + '</time></span>';
				        html += '</div>';
				    }
				    html += '</header>';

				    html += '<div class="bddex-entry-content">';
				    if ( settingsValue.showExcerpt == 1 ) {
				        html += '<div class="bddex-grid-post-excerpt">';
				        html += '<p>' + data[i].excerpt.rendered.split(/\s+/).slice( 0, settingsValue.excerptLength ).join( " " ) + '</p>';
				        html += '</div>';
				    }
				    html += '</div>';
				    html += '</div>';
				    if ( settingsValue.showMeta == 1 && settingsValue.metaPosition == 'meta-entry-footer' ) {
				        html += '<div class="bddex-entry-footer">';
				        html += '<div class="bddex-author-avatar">';
				        html += '<a href="' + data[i]._embedded.author[0].link + '">';
				        html += '<img src="' + data[i]._embedded.author[0].avatar_urls[96] + '" class="avatar avatar-96 photo" />';
				        html += '</a>';
				        html += '</div>';
				        html += '<div class="bddex-entry-meta">';
				        html += '<div class="bddex-posted-by">';
				        html += '<a href="' + data[i]._embedded.author[0].link + '">' + data[i]._embedded.author[0].name + '</a>';
				        html += '</div>';
				        html += '<div class="bddex-posted-on">';
				        html += '<time datetime="' + get_post_date( getPostDate ) + '">' + get_post_date( getPostDate );
				        html += '</time>';
				        html += '</div>';
				        html += '</div>';
				        html += '</div>';
				    }
				    html += '</div>';
				    html += '</article>';
				}
				optionsValue.postContainer.append( html );
			}

		}

	}



	/**
	 * Get Date
	 */
	function get_post_date( date ) {
		var getDate = new Date( date );
		var month = new Array();
		month[0] = "January";
		month[1] = "February";
		month[2] = "March";
		month[3] = "April";
		month[4] = "May";
		month[5] = "June";
		month[6] = "July";
		month[7] = "August";
		month[8] = "September";
		month[9] = "October";
		month[10] = "November";
		month[11] = "December";
		var dayNum = getDate.getDate();
		var monthName = month[ getDate.getMonth() ];
		var getYear = getDate.getFullYear();

		var returnYear = monthName + ' ' + dayNum + ', ' + getYear;
		return returnYear;
	}

})(jQuery);