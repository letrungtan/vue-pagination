<template>
	<div v-if="isShowPaginate">
		<ul class="pagination">
			<li
				class="page-item"
				:class="{disabled: currentPage <= 1}"
			>
				<a
					href="#"
					class="page-link page-link-prev"
					@click.prevent="selectPage(currentPage - 1)"
				>{{prevText}}</a>
			</li>
			<li
				class="page-item"
				v-for="(page, k) in paginate"
			>
				<a
					href="#"
					class="page-link"
					:class="{active: page === currentPage}"
					@click.prevent="selectPage(page)"
				>{{page}}</a>
			</li>
			<li
				class="page-item"
				:class="{disabled: currentPage >= lastPage}"
			>
				<a
					href="#"
					class="page-link page-link-next"
					@click.prevent="selectPage(currentPage + 1)"
				>{{nextText}}</a>
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	props: {
		currentPage: { default: () => 1, type: Number },
		perPage: { default: () => 1, type: Number },
		totalItems: { default: () => 0, type: Number },
		prevText: { default: () => 'Prev', type: String },
		nextText: { default: () => 'Next', type: String },
		maxSize: { default: () => 5, type: Number },
	},
	'data' () {
		return {
			paginate: [],
			isShowPaginate: false
		}
	},
	computed: {
		lastPage() {
			if (this.totalItems % this.perPage === 0) {
				return this.totalItems / this.perPage;
			}
			return Math.floor(this.totalItems / this.perPage) + 1;
		}
	},
	methods: {
		selectPage (page) {
			if (page < 1 || page > this.lastPage) {
				return;
			}
			this.$emit('pageSelected', page);
		},
		init () {
			let maxSize = this.maxSize;
			var currentPage = Number(this.currentPage);
			console.log('currentPage', currentPage);
			var lastPage =  Number(this.lastPage);
			(maxSize > lastPage)? maxSize = lastPage : '';
			var pos = Math.floor( maxSize / 2 ) + 1;
			var paginate = [];
			for (var i = 0; i < maxSize; i++) {
				paginate[i] =  i+1;
			}
			if (currentPage >= pos){
				for (var i = 0; i < maxSize; i++) {
					paginate[i] = currentPage + i - pos + 1;
				}
			}
			if (currentPage >= pos && currentPage > lastPage-pos + 1) {
				for (var i = 0; i < maxSize; i++) {
					paginate[i] = lastPage - maxSize + i + 1;
				}
			}
			console.log('paginate', paginate);
			this.paginate = paginate;
		},
		checkShowPaginate() {
			if (this.totalItems > this.perPage) {
				this.isShowPaginate = true;
			} else {
				this.isShowPaginate = false;
			}
		}
	},
	mounted () {
		this.init();
		this.checkShowPaginate();
	},
	watch: {
		'currentPage'() {
			this.init();
		},
		totalItems() {
			this.init();
			this.checkShowPaginate();
		},
		'perPage'() {
			this.init();
			this.checkShowPaginate();
		}
	}
}
</script>

<style>
.pagination {
    margin: 15px 0 !important;
}

.pagination {
    display: inline-block;
    padding-left: 0;
    margin: 20px 0;
    border-radius: 4px;
}
.pagination>li {
    display: inline;
}

.pagination>li:first-child>a, .pagination>li:first-child>span {
    margin-left: 0;
    border-bottom-left-radius: 1px !important;
    border-top-left-radius: 1px !important;
}

.pagination>li:last-child>a, .pagination>li:last-child>span {
    border-bottom-right-radius: 1px !important;
    border-top-right-radius: 1px !important;
}

.pagination>.disabled>span,
.pagination>.disabled>span:hover,
.pagination>.disabled>span:focus,
.pagination>.disabled>a,
.pagination>.disabled>a:hover,
.pagination>.disabled>a:focus {
    color: #999;
    background-color: #fff;
    border-color: #ddd;
    cursor: not-allowed !important;
	pointer-events: all !important;
}
.pagination>li>a, .pagination>li>span {
    position: relative;
    float: left;
    padding: 6px 12px;
    line-height: 1.428571429;
    text-decoration: none;
    color: #428bca;
    background-color: #fff;
    border: 1px solid #ddd;
    margin-left: -1px;
}
.page-link:focus {
    z-index: 2;
    outline: 0;
    box-shadow: none !important;
}
.pagination .active,
.pagination .active:hover,
.pagination .active:focus,
.pagination .active:focus {
    z-index: 2;
    color: #fff;
    background-color: #428bca;
    border-color: #428bca;
    cursor: default;
}

.pagination .page-link-prev, .pagination .page-link-next {
    color: #167495;
    background-color: #fff;
	border-color: #ddd;;
}
</style>
