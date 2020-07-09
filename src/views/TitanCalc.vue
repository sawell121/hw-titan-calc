<template>
	<div>
		<div class="bg-gray-800 pb-32">
			<nav class="bg-gray-800"></nav>
			<header class="py-10">
				<div class="mx-auto px-4 sm:px-4 lg:px-8">
					<h1 class="text-3xl leading-9 font-bold text-white">
						Titan Potions

						<div class="flex flex-row">
							<a href="#" class="text-sm font-thin" @click="saveIt">
								(Save)
							</a>

							<a href="#" class="text-sm font-thin ml-3" @click="clearIt">
								(Clear)
							</a>
						</div>
					</h1>
				</div>
			</header>
		</div>

		<div class="-mt-32 flex flex-row">
			<div class="bg-white shadow rounded-lg flex flex-col">
				<table>
					<thead class="">
						<tr>
							<th
								class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
							>
								Titan Name
							</th>
							<th
								class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
								style="width: 120px;"
							>
								Level
							</th>
							<th
								class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
							>
								Emeralds Needed to Max
							</th>
							<th
								class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
							>
								Potions Needed to Max
							</th>
							<th
								class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
							>
								in team
							</th>
						</tr>
					</thead>
					<tbody class="bg-white">
						<tr v-for="(titan, index) in titans" :key="titan.id">
							<td class="px-4 py-1 border-b border-gray-200">
								<div class="flex items-center">
									<div class="flex-shrink-0 h-10 w-10">
										<img class="h-8 w-8 rounded-full" :src="getTitanIcon(titan.id)" alt="" srcset="" />
									</div>
									<div class="ml-4">
										<div class="text-sm leading-5 font-medium text-gray-900">
											{{ localeData[`LIB_HERO_NAME_${titan.id}`] }}
										</div>
									</div>
								</div>
							</td>
							<td class="px-4 py-1 border-b border-gray-200">
								<div class="max-w-lg rounded-md shadow-sm">
									<input
										type="number"
										@input="maxLengthCheck($event, index)"
										:max="120"
										:min="0"
										:id="titan.id"
										v-model="titan.level"
										class="form-input block w-full transition duration-150 ease-in-out sm:text-sm sm:leading-5"
									/>
								</div>
							</td>

							<td v-if="computeCosts && computeCosts[index]" class="px-4 py-1 border-b border-gray-200">
								{{ computeCosts[index].emeralds_needed ? numberWithCommas(computeCosts[index].emeralds_needed) : 0 }}
							</td>
							<td v-if="computeCosts && computeCosts[index]" class="px-4 py-1 border-b border-gray-200">
								{{ computeCosts[index].potions_needed ? numberWithCommas(computeCosts[index].potions_needed) : 0 }}
							</td>
							<td class="px-4 py-1 border-b border-gray-200">
								<fieldset>
									<div class="mt-4">
										<div class="relative flex items-start">
											<div class="absolute flex items-center h-5">
												<input
													:disabled="selectedTitansCost.length > 4 && !titan.inTeam"
													id="comments"
													@click="titan.inTeam = !titan.inTeam"
													type="checkbox"
													class="form-checkbox h-4 w-4 text-indigo-600 transition duration-150 ease-in-out"
												/>
											</div>
											<div class="pl-7 text-sm leading-5">
												<label
													for="comments"
													class="font-medium text-gray-700"
													:class="selectedTitansCost.length > 4 ? 'text-gray-700' : 'text-gray-100'"
													>{{ titan.inTeam ? "Yes" : "No" }}</label
												>
											</div>
										</div>
									</div>
								</fieldset>
							</td>
						</tr>
					</tbody>
				</table>
			</div>
			<div class="flex flex-col h-20" style="min-width: 300px;">
				<div class="mx-3 bg-white shadow rounded-lg">
					<div class="p-2">
						<dl class="flex flex-row items-center justify-between">
							<img class="h-8 w-8 rounded-full" src="@/assets/titan_potion.png" alt="" srcset="" />
							<dd class="mt-1 text-2xl leading-9 font-semibold text-gray-900 mr-3">
								{{ numberWithCommas(totalPotionsNeeded) }}
							</dd>
						</dl>
					</div>
				</div>
				<div class="mx-3 mt-2 bg-white shadow rounded-lg">
					<div class="p-2">
						<dl class="flex flex-row items-center justify-between">
							<dt class="text-sm font-medium text-gray-500 truncate">
								<img class="h-8 w-8 rounded-full" src="@/assets/emeralds.png" alt="" srcset="" />
							</dt>
							<div class="row">
								<dd class="mt-1 text-2xl leading-9 font-semibold text-gray-900 mr-3">
									{{ numberWithCommas(totalEmeraldsNeeded) }}
								</dd>
							</div>
						</dl>
					</div>
				</div>
			<div v-for="(titan, index) in selectedTitans" :key="titan.id" class="mx-2 bg-white shadow rounded-lg mt-2">
				<div class="flex flex-rowshadow-md rounded-md py-4 pl-2 justify-between">
					<div class="flex flex-row justify-center align-center align-center items-center ">
						<img class="h-10 w-10 rounded-full" :src="getTitanIcon(titan.id)" alt="" srcset="" />
						<p class="pl-2">{{ localeData[`LIB_HERO_NAME_${titan.id}`] }}</p>
					</div>
					<div class="flex flex-row">
						<dl>
							<dd class="flex flex-col mr-2">
								<div class="flex flex-row">
									<img class="h-5 w-5 rounded-full" src="@/assets/emeralds.png" alt="" srcset="" />
									{{ numberWithCommas(selectedTitansCost[index].emeralds_needed) }}
								</div>
								<div class="flex flex-row">
									<img class="h-5 w-5 rounded-full" src="@/assets/titan_potion.png" alt="" srcset="" />
									{{ numberWithCommas(selectedTitansCost[index].potions_needed) }}
								</div>
							</dd>
						</dl>
					</div>
				</div>
			</div>
		</div>
		</div>
		
	</div>
</template>

<script>
// @ is an alias to /src
import serverDefendingTeam from "@/data/war_defence.js"
import guildMembers from "@/data/member_info.js"
import moment from "moment"

let lib = require("@/data/lib.json")
let locale = require("@/data/en.json")
let titanLevelData = require("@/data/titan_levels.json")

export default {
	name: "Home",
	data() {
		return {
			guildLocalDefence: serverDefendingTeam.results[0].result.response.warriors,
			guildLocalDefenceTeams: Object.values(serverDefendingTeam.results[0].result.response.teams),
			guildMembers: Object.values(guildMembers.results[0].result.response.clan.members),
			libData: lib,
			localeData: locale,
			titanLevelData,
			titans: [
				{ id: 4000, level: 0, inTeam: false },
				{ id: 4001, level: 0, inTeam: false },
				{ id: 4002, level: 0, inTeam: false },
				{ id: 4003, level: 0, inTeam: false },
				{ id: 4010, level: 0, inTeam: false },
				{ id: 4011, level: 0, inTeam: false },
				{ id: 4012, level: 0, inTeam: false },
				{ id: 4013, level: 0, inTeam: false },
				{ id: 4020, level: 0, inTeam: false },
				{ id: 4021, level: 0, inTeam: false },
				{ id: 4022, level: 0, inTeam: false },
				{ id: 4023, level: 0, inTeam: false },
			],
			titansClean: [
				{ id: 4000, level: 0, inTeam: false },
				{ id: 4001, level: 0, inTeam: false },
				{ id: 4002, level: 0, inTeam: false },
				{ id: 4003, level: 0, inTeam: false },
				{ id: 4010, level: 0, inTeam: false },
				{ id: 4011, level: 0, inTeam: false },
				{ id: 4012, level: 0, inTeam: false },
				{ id: 4013, level: 0, inTeam: false },
				{ id: 4020, level: 0, inTeam: false },
				{ id: 4021, level: 0, inTeam: false },
				{ id: 4022, level: 0, inTeam: false },
				{ id: 4023, level: 0, inTeam: false },
			],
			memberData: {
				name: "Seawall",
				userId: "30316421",
				lastLogin: "18 days ago",
				level: "120",
				isChampion: true,
				clanDefence_titans: [
					{ id: 4000, level: 0, star: 0, power: 0 },
					{ id: 4001, level: 0, star: 0, power: 0 },
					{ id: 4002, level: 0, star: 0, power: 0 },
					{ id: 4003, level: 0, star: 0, power: 0 },
					{ id: 4010, level: 0, star: 0, power: 0 },
					{ id: 4011, level: 0, star: 0, power: 0 },
					{ id: 4012, level: 0, star: 0, power: 0 },
					{ id: 4013, level: 0, star: 0, power: 0 },
					{ id: 4020, level: 0, star: 0, power: 0 },
					{ id: 4021, level: 0, star: 0, power: 0 },
					{ id: 4022, level: 0, star: 0, power: 0 },
					{ id: 4023, level: 0, star: 0, power: 0 },
				],
				clanDefence_heroes: [
					{ id: 24, level: 120, color: 14, star: 6, power: 55902 },
					{ id: 33, level: 120, color: 15, star: 6, power: 82184 },
					{ id: 34, level: 120, color: 14, star: 6, power: 64258 },
					{ id: 35, level: 120, color: 13, star: 6, power: 41220 },
					{ id: 46, level: 120, color: 14, star: 5, power: 42961 },
				],
				totalTitanPower: 192160,
				totalHeroPower: 286525,
			},
		}
	},
	methods: {
		saveIt() {
			localStorage.setItem("titan_data", JSON.stringify(this.titans))
		},
		clearIt() {
			localStorage.clear()
			this.titans = this.titansClean
		},
		maxLengthCheck(evt, index) {
			console.log(evt)
			if (evt > 120) this.titans[index].level = 120
		},
		numberWithCommas(x) {
			return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
		},
		zeroPad(input, length) {
			return (Array(length + 1).join("0") + input).slice(-length)
		},
		getTitanIcon(id) {
			return require("@/assets/titan_hero_icons/" + id + ".png")
		},
	},
	mounted() {
		const cachedTitans = localStorage.getItem("titan_data")
		if (cachedTitans) this.titans = JSON.parse(cachedTitans)
	},
	computed: {
		totalPotionsNeeded() {
			if (this.computeCosts && this.computeCosts.length > 0) {
				return this.computeCosts.reduce((x, z) => x + z.potions_needed, 0)
			}
		},
		totalEmeraldsNeeded() {
			if (this.computeCosts && this.computeCosts.length > 0) {
				return this.computeCosts.reduce((x, z) => x + z.emeralds_needed, 0)
			}
		},
		selectedTitansCost() {
			let totalPotions = 0
			let titanArray = []
			let titanLevelData = this.titanLevelData
			if (this.selectedTitans && this.selectedTitans.length > 0) {
				this.selectedTitans.forEach((titan) => {
					titanArray.push({
						emeralds_needed: titanLevelData
							.filter((ti) => ti.level > titan.level)
							.reduce((x, z) => x + z.emerald_cost, 0),
						potions_needed: titanLevelData
							.filter((ti) => ti.level > titan.level)
							.reduce((x, z) => x + z.potion_cost, 0),
					})
				})
			}
			return titanArray
		},
		computeCosts() {
			let totalPotions = 0
			let titanArray = []
			let titanLevelData = this.titanLevelData
			this.titans.forEach((titan) => {
				titanArray.push({
					emeralds_needed: titanLevelData
						.filter((ti) => ti.level > titan.level)
						.reduce((x, z) => x + z.emerald_cost, 0),
					potions_needed: titanLevelData.filter((ti) => ti.level > titan.level).reduce((x, z) => x + z.potion_cost, 0),
				})
			})

			return titanArray
		},
		selectedTitans() {
			return this.titans.filter((titan) => titan.inTeam)
		},
		computeHeroes() {
			return Object.values(this.libData.hero).map((hero) => {
				return {
					...hero,
					name: this.localeData[`LIB_HERO_NAME_${hero.id}`],
				}
			})
		},
		computeMembers() {
			const memberValues = this.guildMembers
			const teamValues = this.guildLocalDefenceTeams
			const championValues = Object.keys(this.guildLocalDefence)

			let newArr = []

			memberValues.forEach((member) => {
				let newObj = {
					name: member.name,
					userId: member.id,
					lastLogin: moment.unix(member.lastLoginTime).fromNow(),
					level: member.level,
					isChampion: false,
				}
				teamValues.forEach((team) => {
					let userid = String(team.userId)
					let memberid = String(member.id)
					if (userid === memberid) {
						let totalTitanPower = 0
						let totalHeroPower = 0

						Object.values(team.clanDefence_titans.units).forEach((titan) => {
							totalTitanPower = totalTitanPower + titan.power
						})
						Object.values(team.clanDefence_heroes.units).forEach((hero) => {
							totalHeroPower = totalHeroPower + hero.power
						})
						newObj.clanDefence_titans = Object.values(team.clanDefence_titans.units)
						newObj.clanDefence_heroes = Object.values(team.clanDefence_heroes.units)
						newObj.totalTitanPower = totalTitanPower
						newObj.totalHeroPower = totalHeroPower
					}
				})
				championValues.forEach((champ) => {
					if (champ === member.id) {
						newObj.isChampion = true
					}
				})
				newArr.push(newObj)
			})

			return newArr.sort(function(a, b) {
				return b.totalTitanPower - a.totalTitanPower
			})
		},
	},
}
</script>
