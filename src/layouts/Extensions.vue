<template>
	<div v-if="extensions.length > 0" class="extension-list">
		<p>List of available extensions to use with Tachiyomi, you can download them from here or from the app.</p>
		<div v-for="extensionGroup in extensions" :key="extensionGroup[0].lang">
			<h3>
				{{
					extensionGroup[0].lang === "en"
						? simpleLangName(extensionGroup[0].lang)
						: langName(extensionGroup[0].lang)
				}}
				<span class="extensions-total">
					Total:
					<span class="extensions-total-sum">
						{{ extensions.reduce((sum, item) => sum + item.length, 0) }}
					</span>
				</span>
			</h3>
			<div
				v-for="extension in extensionGroup"
				:id="extension.pkg.replace('eu.kanade.tachiyomi.extension.', '')"
				:key="extension.apk"
				class="anchor"
			>
				<div class="extension">
					<a
						:href="`#${extension.pkg.replace('eu.kanade.tachiyomi.extension.', '')}`"
						@click.stop
						aria-hidden="true"
						class="header-anchor"
					>
						#
					</a>
					<img class="extension-icon" :src="iconUrl(extension.apk)" width="42" height="42" />
					<div class="extension-text">
						<div class="upper">
							<span class="font-semibold">{{ extension.name.split(": ")[1] }}</span>
							<Badge :text="'v' + extension.version" />
						</div>
						<div class="down">
							{{ extension.pkg.replace("eu.kanade.tachiyomi.extension.", "") }}
						</div>
					</div>
					<a :href="apkUrl(extension.apk)" class="extension-download" title="Download APK" download>
						<DownloadIcon class="inline" size="1x" />
						<span>Download</span>
					</a>
				</div>
			</div>
		</div>
	</div>
	<div v-else class="extension-list">
		<p>List of available extensions to use with Tachiyomi, you can download them from here or from the app.</p>
		<div class="circle-loader"></div>
	</div>
</template>

<script>
import ISO6391 from "iso-639-1";
import { DownloadIcon } from "vue-feather-icons";

export default {
	components: {
		DownloadIcon,
	},

	data() {
		return {
			extensions: [],
		};
	},
	mounted() {
		this.$store.dispatch("fetchExtensions").then(data => {
			const values = Object.values(data);
			values.sort((a, b) => {
				const langA = this.simpleLangName(a[0].lang);
				const langB = this.simpleLangName(b[0].lang);
				if (langA === "All" && langB === "English") {
					return -1;
				}
				if (langA === "English" && langB === "All") {
					return 1;
				}
				if (langA === "English") {
					return -1;
				}
				if (langB === "English") {
					return 1;
				}
				if (langA < langB) {
					return -1;
				}
				if (langA > langB) {
					return 1;
				}
				return 0;
			});
			this.$data.extensions = values;
		});
	},

	updated() {
		if (window.location.hash) {
			window.location.replace(window.location.hash);
		}
	},

	methods: {
		simpleLangName: code => (code === "all" ? "All" : ISO6391.getName(code)),
		langName: code => (code === "all" ? "All" : `${ISO6391.getName(code)} (${ISO6391.getNativeName(code)})`),
		iconUrl(pkg) {
			const pkgName = pkg.substring(0, pkg.lastIndexOf("."));
			return `https://raw.githubusercontent.com/tachiyomiorg/tachiyomi-extensions/repo/icon/${pkgName}.png`;
		},
		apkUrl: apk => `https://raw.githubusercontent.com/tachiyomiorg/tachiyomi-extensions/repo/apk/${apk}`,
	},
};
</script>

<style lang="stylus">
.extension-list {
	h3 {
		padding-bottom 0.75em
		border-bottom 1px solid var(--border)
	}

	> div {
		&:not(:first-of-type) {
			.extensions-total {
				display none
			}
		}
	}
}

.extensions-total {
	float right

	&-sum {
		color var(--primary)
	}
}

.anchor {
	margin-top -3.9em
	padding-bottom 0.2em
	padding-top 4.5em

	.extension {
		align-items center
		display flex
		padding 0.4em 0.2em

		.header-anchor {
			padding-left 0.2em
			padding-right 0.2em
			font-size 1.4em
			opacity 0
		}

		&:hover .header-anchor {
			opacity 1
		}

		.extension-icon {
			margin-right 0.5em
		}

		.extension-text {
			flex 1

			.upper {
				.badge {
					margin-left 8px
				}
			}

			.down {
				color #6c757d
				font-family monospace
				font-size 0.9rem
			}
		}

		.extension-download {
			color var(--primary) !important
			font-weight 700
			border-color var(--border)
			border-width 1px
			border-radius 0.5rem
			margin-right 0.5em
			padding-top 0.5rem
			padding-bottom 0.5rem
			padding-left 1rem
			padding-right 1rem
			transition-property background-color, border-color, color, fill, stroke

			&:hover {
				background-color var(--primary)
				color #ffffff !important
			}

			span {
				margin-left 0.5rem
			}

			svg {
				margin-bottom 4px
				display inline-block
			}
		}

		@media (max-width 767px) {
			.extension-text .down,
			.extension-download span {
				display none
			}
		}
	}

	@media (max-width 767px) {
		.extension {
			border 1px solid var(--border)
			border-radius 8px

			.extension-download {
				background-color var(--container)
			}
		}
	}

	&:target {
		.extension {
			background-color var(--container)
			border-radius 8px
			transition 500ms background-color

			.extension-download {
				background-color var(--background)

				&:hover {
					background-color var(--primary)

					span {
						color white
					}
				}
			}
		}
	}

	&:first-child {
		border-top 1px solid var(--border)
	}
}

.circle-loader {
	border 8px solid #efefef
	border-top 8px solid var(--primary)
	border-radius 99em
	width 3em
	height 3em
	animation spin 2s linear infinite
	margin 0 auto
}

@keyframes spin {
	from {
		transform rotate(0deg)
	}

	to {
		transform rotate(360deg)
	}
}
</style>
