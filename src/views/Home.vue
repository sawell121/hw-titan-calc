<template>
	<div>
		<div class="bg-gray-800 pb-32">
			<nav class="bg-gray-800">
				<div class="mx-auto sm:px-4 lg:px-8">
					<div class="border-b border-gray-700">
						<div class="flex items-center justify-between h-16 px-4 sm:px-0">
							<div class="flex items-center">
								<div class="hidden md:block">
									<div class="flex items-baseline">
										<a
											href="#"
											class="px-3 py-2 rounded-md text-sm font-medium text-white bg-gray-900 focus:outline-none focus:text-white focus:bg-gray-700"
											>Members
										</a>
										<a
											href="#"
											class="ml-4 px-3 py-2 rounded-md text-sm font-medium text-gray-300 hover:text-white hover:bg-gray-700 focus:outline-none focus:text-white focus:bg-gray-700"
											>War Calculator
										</a>
										<a
											href="#"
											class="ml-4 px-3 py-2 rounded-md text-sm font-medium text-gray-300 hover:text-white hover:bg-gray-700 focus:outline-none focus:text-white focus:bg-gray-700"
											>Team Calculator
										</a>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
			</nav>
			<header class="py-10">
				<div class="mx-auto px-4 sm:px-4 lg:px-8">
					<h1 class="text-3xl leading-9 font-bold text-white">
						Members
					</h1>
				</div>
			</header>
		</div>

		<main class="-mt-32">
			<div class=" mx-auto pb-12 px-4 sm:px-4 lg:px-8">
				<div class="">
					<div v-if="computeMembers" class="border-gray-200 rounded-lg h-96">
						<div>
							<div class="bg-white shadow overflow-hidden sm:rounded-md">
								<ul>
									<li v-for="member in computeMembers" :key="member.id">
										<a
											href="#"
											class="block hover:bg-gray-50 focus:outline-none focus:bg-gray-50 transition duration-150 ease-in-out"
										>
											<div class="flex items-center px-4 py-1 sm:px-4">
												<div class="flex-1 flex items-center">
													<div class="flex-1 px-6 pt-3 md:grid md:grid-cols-3 md:gap-4">
														<div>
															<div class="text-xl leading-5 font-medium text-red-600 truncate">
																{{ member.name }}
															</div>
															<div class="mt-2 flex items-center text-sm leading-5 text-gray-500">
																<span class="truncate">last seen {{ member.lastLogin }} </span>
															</div>
                              <div class="flex flex-col justify-center py-4">
                                <div class="mb-2"
																							
																						>
                                            <span class="p-1 px-3 text-sm leading-5 rounded-full rounded-l-none bg-teal-100 text-green-800">Hero Power: {{ numberWithCommas(member.totalHeroPower) }}</span>
																							
																						</div>
                                            <div
																						>
                                            <span class="p-2 px-3 inline-flex text-md leading-5 font-semibold rounded-l-none rounded-full bg-orange-100 text-green-800">Titan Power: {{ numberWithCommas(member.totalTitanPower) }}</span>
																							
																						</div>
                              </div>
														</div>
                            <div v-if="member.clanDefence_heroes">
															<div class="flex flex-col">
																<div class="-my-2 py-2 overflow-x-auto sm:-mx-6 sm:px-4 lg:-mx-8 lg:px-8">
																	<div
																		class="align-middle inline-block min-w-full shadow overflow-hidden sm:rounded-lg border-b border-gray-200"
																	>
																		<table class="min-w-full">
																			<thead>
																				<tr>
																					<th
																						class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
																					>
																						Hero Name
																					</th>
                                          <th
																						class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
																					>
																						Power
																					</th>
																					<th
																						class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
																					>
																						Level
																					</th>
																					<th
																						class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
																					>
																						Star
																					</th>
																				</tr>
																			</thead>
																			<tbody class="bg-white">
																				<tr v-for="titan in member.clanDefence_heroes" :key="titan.id">
																					<td class="px-4 py-1 whitespace-no-wrap border-b border-gray-200">
																						<div class="flex items-center">
																							<div class="flex-shrink-0 h-10 w-10">
																								<img
																									class="h-8 w-8 rounded-full"
																									:src="getTitanIcon(titan.id)"
																									alt=""
																									srcset=""
																								/>
																							</div>
																							<div class="ml-4">
																								<div class="text-sm leading-5 font-medium text-gray-900">
																									{{ localeData[`LIB_HERO_NAME_${titan.id}`] }}
																								</div>
																							</div>
																						</div>
																					</td>
                                          <td class="px-4 py-1 whitespace-no-wrap border-b border-gray-200">
																						<div class="text-sm leading-5 text-gray-900">{{ numberWithCommas(titan.power) }}</div>
																					</td>
																					<td class="px-4 py-1 whitespace-no-wrap border-b border-gray-200">
																						<div class="text-sm leading-5 text-gray-900">{{ titan.level }}</div>
																					</td>
																					<td class="px-4 py-1 whitespace-no-wrap border-b border-gray-200">
																						<span
																							class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800"
																						>
																							{{ titan.star }}
																						</span>
																					</td>
																				</tr>
																			</tbody>
																		</table>
																	</div>
																</div>
															</div>
														</div>
														<div v-if="member.clanDefence_titans">
															<div class="flex flex-col">
																<div class="-my-2 py-2 overflow-x-auto sm:-mx-6 sm:px-4 lg:-mx-8 lg:px-8">
																	<div
																		class="align-middle inline-block min-w-full shadow overflow-hidden sm:rounded-lg border-b border-gray-200"
																	>
																		<table class="min-w-full">
																			<thead>
																				<tr>
																					<th
																						class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
																					>
																						Titan Name
																					</th>
                                          <th
																						class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
																					>
																						Power
																					</th>
																					<th
																						class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
																					>
																						Level
																					</th>
																					<th
																						class="px-4 py-3 border-b border-gray-200 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
																					>
																						Star
																					</th>
																				</tr>
																			</thead>
																			<tbody class="bg-white">
																				<tr v-for="titan in member.clanDefence_titans" :key="titan.id">
																					<td class="px-4 py-1 whitespace-no-wrap border-b border-gray-200">
																						<div class="flex items-center">
																							<div class="flex-shrink-0 h-10 w-10">
																								<img
																									class="h-8 w-8 rounded-full"
																									:src="getTitanIcon(titan.id)"
																									alt=""
																									srcset=""
																								/>
																							</div>
																							<div class="ml-4">
																								<div class="text-sm leading-5 font-medium text-gray-900">
																									{{ localeData[`LIB_HERO_NAME_${titan.id}`] }}
																								</div>
																							</div>
																						</div>
																					</td>
                                          <td class="px-4 py-1 whitespace-no-wrap border-b border-gray-200">
																						<div class="text-sm leading-5 text-gray-900">{{ numberWithCommas(titan.power) }}</div>
																					</td>
																					<td class="px-4 py-1 whitespace-no-wrap border-b border-gray-200">
																						<div class="text-sm leading-5 text-gray-900">{{ titan.level }}</div>
																					</td>
																					<td class="px-4 py-1 whitespace-no-wrap border-b border-gray-200">
																						<span
																							class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800"
																						>
																							{{ titan.star }}
																						</span>
																					</td>
																				</tr>
																			</tbody>
																		</table>
																	</div>
																</div>
															</div>
														</div>
													</div>
												</div>
												<div>
													<svg class="h-5 w-5 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
														<path
															fill-rule="evenodd"
															d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
															clip-rule="evenodd"
														/>
													</svg>
												</div>
											</div>
										</a>
									</li>
								</ul>
							</div>
						</div>
					</div>
				</div>
				<!-- /End replace -->
			</div>
		</main>
	</div>
</template>

<script>
// @ is an alias to /src
import serverDefendingTeam from "@/data/war_defence.js"
import guildMembers from "@/data/member_info.js"
import moment from "moment"

let lib = require("@/data/lib.json")
let locale = require("@/data/en.json")

export default {
	name: "Home",
	data() {
		return {
			guildLocalDefence: serverDefendingTeam.results[0].result.response.warriors,
			guildLocalDefenceTeams: Object.values(serverDefendingTeam.results[0].result.response.teams),
			guildMembers: Object.values(guildMembers.results[0].result.response.clan.members),
			libData: lib,
			localeData: locale,
		}
	},
	methods: {
    numberWithCommas(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
},
		zeroPad(input, length) {
			return (Array(length + 1).join("0") + input).slice(-length)
		},
		getTitanIcon(id) {
			return require("@/assets/titan_hero_icons/" + id + ".png")
		},
	},
	computed: {
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

            Object.values(team.clanDefence_titans.units).forEach(titan => {
              totalTitanPower = totalTitanPower + titan.power
            })
            Object.values(team.clanDefence_heroes.units).forEach(hero => {
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

			return newArr.sort(function (a, b) {
  return b.totalTitanPower - a.totalTitanPower;
});
		},
	},
}
</script>
