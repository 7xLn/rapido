# rapido
#!/bin/bash

#Colours
greenColour="\e[0;32m\033[1m"
endColour="\033[0m\e[0m"
redColour="\e[0;31m\033[1m"
blueColour="\e[0;34m\033[1m"
yellowColour="\e[0;33m\033[1m"
purpleColour="\e[0;35m\033[1m"
turquoiseColour="\e[0;36m\033[1m"
grayColour="\e[0;37m\033[1m"

# Colores básicos
blackColour="\033[0;30m"
whiteColour="\033[1;37m"

# Colores brillantes
brightRedColour="\033[1;31m"
brightGreenColour="\033[1;32m"
brightYellowColour="\033[1;33m"
brightBlueColour="\033[1;34m#"
brightPurpleColour="\033[1;35m"
brightCyanColour="\033[1;36m"
brightWhiteColour="\033[1;37m"

# Colores de fondo
bgBlackColour="\033[40m"
bgRedColour="\033[41m"
bgGreenColour="\033[42m"
bgYellowColour="\033[43m"
bgBlueColour="\033[44m"
bgPurpleColour="\033[45m"
bgCyanColour="\033[46m"
bgWhiteColour="\033[47m"

# Colores subrayados
underlineRedColour="\033[4;31m"
underlineGreenColour="\033[4;32m"
underlineYellowColour="\033[4;33m"
underlineBlueColour="\033[4;34m"
underlinePurpleColour="\033[4;35m"
underlineCyanColour="\033[4;36m"
underlineWhiteColour="\033[4;37m"

# Colores parpadeantes
blinkRedColour="\033[5;31m"
blinkGreenColour="\033[5;32m"
blinkYellowColour="\033[5;33m"
blinkBlueColour="\033[5;34m"
blinkPurpleColour="\033[5;35m"
blinkCyanColour="\033[5;36m"
blinkWhiteColour="\033[5;37m"

# Colores personalizados (mezclas)
orangeColour="\033[38;5;208m"    # Naranja
pinkColour="\033[38;5;205m"      # Rosa
lightBlueColour="\033[38;5;45m"  # Azul claro
darkGreenColour="\033[38;5;22m"  # Verde iscuro

# Reset de color (¡muy importante!)
endColour="\033[0m"

function ctrl_c(){
	echo -e "\n\n${redColour}[!] Saliendo...${endColour}"
	tput cnorm; exit 1
}



trap ctrl_c INT

function helpPanel(){
	echo -e "${yellowColour}[+]${endColour} ${grayColour}Uso${endColour} ${purpleColour}${0}${endColour}\n"
	echo -e "\t${blueColour}-m)${endColour} ${grayColour}Dinero con el que se desea jugar${endColour}"
	echo -e "\t${blueColour}-t)${endColour}${grayColour} Tecnica a utilizar (Martingala-InverseLabruchere)${endColour}"
}

function martingala(){
	echo -e "${yellowColour}[+]${endColour} ${grayColour}Dinero actual:${endColour}${yellowColour} "$money€${endColour}""
	echo -ne "${yellowColour}[+]${endColour} ${grayColour}¿Cuanto dinero deseas apostar? ${endColour}${purpleColour}->${endColour} "
	read initial_bet
	if [[ "$initial_bet" =~ ^[0-9]+(\.[0-9]+)?$ ]]; then
		if ! [[ "$initial_bet" -gt "$money" || "$initial_bet" -eq 0 ]]; then
			echo -ne "${yellowColour}[+]${endColour}${gbrayColour} Deseas jugar a ${yellowColour}par o impar${endColour}${grayColour}?${endColour} ${purpleColour}->${endColour} "
			read par_impar
			par_impar=${par_impar,,}
			if [ $par_impar == "par" ] || [ $par_impar == "impar" ]; then
				if [ "$par_impar" == "par" ]; then
					#todo esto es para PAR
					echo -e "\n${yellowColour}[+]${endColour} ${grayColour}Vamos a jugar con${endColour} ${yellowColour}$initial_bet€${endColour} y a ${yellowColour}$par_impar${endColour}"
				
					backup_bet=$initial_bet
			
						tput civis
					play_counter=0
					numero_de_jugadas_malas=0
					jugadas_malas="[ "
					max_money=0
					while true; do
					if [  "$money" -gt 0 ]; then
						echo -e "\n${bgGreenColour}${yellowColour}[+]${endColour} ${grayColour}Acabas de apostar ${purpleColour}$initial_bet€${endColour} ${grayColour}y tu dinero actual es:${endColour} ${yellowColour}"$money€"${endColour}${endColour}"
						money=$(($money-$initial_bet))
						random_number="$(($RANDOM % 37))"
					#	echo -e "\n${yellowColour}[+]${endColour} ${grayColour}A salido el numero${endColour} ${greenColour}"$random_number${endColour}""
						check_number=$(($random_number % 2))
						if [ "$check_number" -eq 0 ]; then
							if [ ! "$random_number" -eq 0 ]; then	
								#	echo -e "${yellowClour}[+]${endColour} ${grayColour}El numero es par ${brightPurpleColour}!GANASTEE!${endColour}"
								reward=$(($initial_bet * 2 ))
								money=$(($money + $reward))
							#	echo -e "${yellowColour}[+]${endColour} ${grayColour}Haz ganado:${endColour} ${yellowColour}$reward€${yellowColour}"
								echo -e "${yellowColour}[+]${endColour} ${grayColour}tienes ${greenColour}$money€${endColour}"
								initial_bet=$backup_bet
								jugadas_malas="[ "
								if [ "$money" -gt "$max_money" ]; then
									max_money=$money
								fi

							else 	
							#	echo -e "${redColour}[!] A salido el 0 por tanto perdemos${endColour}"

								initial_bet=$(($initial_bet*2))
								echo -e "${yellowColour}[+]${endColour} ${grayColour}Ahora mismo tienes ${endColour}${yellowColour}$money€${endColour}"
								let numero_de_jugadas_malas+=1
								jugadas_malas+="$random_number "
						

							fi
							else
						#	echo -e "${redColour}[!] El numero es impar, haz perdido${endColour}"
							initial_bet=$(($initial_bet*2))
							echo -e "${yellowColour}[+]${endColour} ${grayColour}Ahora mismo tienes ${endColour}${yellowColour}$money€${endColour}"
							let numero_de_jugadas_malas+=1
							jugadas_malas+="$random_number "
							echo -e "\n${yellowColour}[+]${endColour} ${grayColour}han habido un total de${endColour} ${greenColour}$numero_de_jugadas_malas${endColour} ${grayColour}jugadas malas${endColour}"
							echo -e "${yellowColour}[+]${endColour}${grayColour} Los numeros malos que han salido son${endColour}${yellowColour} $jugadas_malas]${endColour}"

						fi 
					else
					echo  -e "${redColour}\n[!] Te quedaste sin dinero, vete a chambear${endColour}"
					echo -e "${yellowColour}[+]${endColour} ${grayColour}ha habido un total de ${endColour}${yellowColour}$play_counter${endColour}${grayColour} jugadas${endColour}\n"
					echo -e "${yellowColour}[+]${endColour} ${grayColour}El maximo dinero que alcanzaste a tener fue de: ${endColour}${yellowColour}$max_money€${endColour}"
						tput cnorm
						exit 0
					fi
					let play_counter+=1
				
				done
				else
					echo -e "\n${yellowColour}[+]${endColour}${grayColour} Vamos a jugar con ${endColour}${greenColour}$initial_bet€${endColour} ${grayColour}y a${endColour}${purpleColour} $par_impar${endColour}"
					if [ $par_impar == "impar" ]; then
						tput civis
						play_counter=0
						backup_bet=$initial_bet
						jugadas_malas="[ "
						max_money=0
						while true; do
						if [ $money -gt 0 ]; then
						echo -e "\n${bgGreenColour}${yellowColour}[+]${endColour} ${grayColour}Acabas de apostar ${purpleColour}$initial_bet€${endColour} ${grayColour}y tu dinero actual es:${endColour} ${yellowColour}"$money€"${endColour}${endColour}"
						money=$(($money-$initial_bet))
						random_number=$(($RANDOM % 37))
						echo -e "${yellowColour}[+]${endColour} ${grayColour}A salido el numero${endColour}${yellowColour} $random_number${endColour}"
						check_number=$(($random_number % 2))
							if [ $random_number -eq 0 ]; then
								echo -e "${redColour}[!] A salido 0 por tanto perdemos${endColour}"
								initial_bet=$(($initial_bet*2))
								echo -e "${yellowColour}[+]${endColour}${grayColour} Tu dinero se queda en${endColour}${yellowColour} $money${endColour}"
								jugadas_malas+=" $random_number "
								echo -e "${yellowColour}[+]${endColour}${grayColour} Los numeros malos que han salido son${endColour}${yellowColour} $jugadas_malas]${endColour}"

							else
								if [ "$check_number" -eq 1 ]; then
									echo -e "${yellowColour}[+]${endColour}${grayColour} El numero es impar, por tanto${endColour} ${purpleColour}!GANAMOS!${endColour}"
									reward=$(($initial_bet*2))
									money=$(($reward+$money))
									echo -e "${yellowColour}[+]${endColour}${grayColour} Tu dinero se queda entonces con${endColour}${yellowColour} $money ${endColour}"
									initial_bet=$backup_bet
									jugadas_malas=""
									if [ $money -gt $max_money ];then
										max_money=$money
									fi
								else
									echo -e "${redColour}[!] El numero que salio es par por tanto perdemos${endColour}"
									initial_bet=$(("$initial_bet"*2))
									echo -e "${yellowColour}[+]${endColour}${grayColour} Tu dinero se queda en${endColour}${yellowColour} $money${endColour}"
									jugadas_malas+=" $random_number "
									echo -e "${yellowColour}[+]${endColour}${grayColour} Los numeros malos que han salido son${endColour}${yellowColour} $jugadas_malas]${endColour}"

								fi
								#sleep 1
						
							fi
							else
							echo -e "${redColour}[!] Nos quedamos sin dinero, por tanto hemos perdido${endColour}"
							echo -e "${yellowColour}[+]${endColour} ${grayColour}ha habido un total de ${endColour}${yellowColour}$play_counter${endColour}${grayColour} jugadas${endColour}\n"
							echo -e "${yellowColour}[+]${endColour} ${grayColour}El maximo dinero que alcanzaste a tener fue de: ${endColour}${yellowColour}$max_money€${endColour}"
							tput cnorm
							exit 0
						fi
						let play_counter+=1
						done
						
					fi

				fi
			else
				echo -e "\n${redColour}[!] Debe ser par o impar${endColour}\n"
				helpPanel
			fi
		else	
		
			echo -e "\n${redColour}[!] No puedes utilizar ni 0 ni una apuesta mayor a tu dinero${endColour}\n"	
				helpPanel
	
		fi
	else
		echo -e "\n${redColour}[+] Asegurate de poner bien el dinero${endColour}\n"
		helpPanel
	fi

}

function labouchereInverse(){
	if [ "$money" -gt 0 ]; then 
		echo -e "${yellowColour}[+]${endColour} ${grayColour}Dinero actual:${endColour} ${yellowColour}$money ${endColour}"
		echo -ne "${yellowColour}[+]${endColour} ${grayColour}Con cuanto dinero quieres empezar a jugar? ${endColour}${purpleColour}-> ${endColour}"
		read initial_bet

		if [[ "$initial_bet" =~ ^[0-9]+(\.[0-9]+)?$ ]]; then
			if ! [[  "$initial_bet" -gt $money ||  "$initial_bet" -le 0 ]]; then
				declare -a secuencia
				echo -ne "${yellowColour}[+]${endColour} ${grayColour}Con que secuencia quieres jugar?${endColour}${purpleColour} -> ${endColour}"
				read -a secuencia
				secuencia_renew=(${secuencia[@]})
				for num in "${secuencia[@]}"; do
					((suma_de_secuencias += $num))
				done
				if [ $suma_de_secuencias -eq $initial_bet ]; then
					echo -ne "${yellowColour}[+]${endColour}${grayColour} Deseas jugar a par o impar${endColour}${purpleColour} -> ${endColour}"
					read par_impar
					tput civis
					par_impar=${par_impar,,}
					bet_to_renew=$(($money+50))
				#
					echo -e "${yellowColour}[+]${endColour} ${grayColour}El tope de la secuencia se ha definido para una ves que llegemos a${endColour} ${greenColour}$bet_to_renew${endColour}"
					while true; do
						let jugadas_totales+=1
						random_number=$(( $RANDOM % 37 ))
						if [ ! $money -lt 0 ]; then
							if [ ! $money -lt $(($bet_to_renew-100)) ]; then
								if [ ! $money -gt $bet_to_renew ]; then
										if [[ $par_impar == "par" ]] || [[ $par_impar == "impar" ]]; then
											#if terminado
											numero_de_secuencias=${#secuencia[*]}
											if [[ "$numero_de_secuencias" -ne 0  &&  $money -ge 0 ]]; then
												echo -e "\n${yellowColour}[+]${endColour}${grayColour} Comenzamos con la secuencia${endColour}${greenColour} [${secuencia[@]}]${endColour}"
												#if terminaod
												if [ "$par_impar" == "par" ]; then

													echo -e "${yellowColour}[+]${endColour}${grayColour} Ha salido el numero${endColour} ${purpleColour}$random_number${endColour}"
													check_number=$(($random_number % 2 ))
													#comprueba que sea par		comprueba que sea distinto de 0 random_number porque si no es como si tocara impar
													#if terminado
													if [ "$random_number" -eq 0 ]; then
														echo -e "${redColour}[+] A salido el 0 por tanto pierdes${endColour}" 
														bet=$((${secuencia[0]} + ${secuencia[-1]} ))
														let money-=$bet
														if [ "$money" -lt 0 ]; then
															echo -e "\n${redColour}[+] Nos quedamos sin money, a chambear papa${endColour}"
															echo -e "${yellowColour}[+]${endColour} ${grayColour}El numero de jugadas totales, son${endColour} ${purpleColour}$jugadas_totales${endColour}\n"
															exit 0
														fi
														unset secuencia[0]
														unset secuencia[-1] 2>/dev/null
														secuencia=(${secuencia[@]})
														echo -e "${yellowColour}[+]${endColour}${grayColour} Invertimos${endColour} ${greenColour}$bet${endColour}"
														echo -e "${yellowColour}[+]${endColour}${grayColour} Tienes ${endColour} ${yellowColour}$money${endColour}"
														echo -e "${yellowColour}[+]${endColour}${grayColour} La secuencia se nos quedaria de la siguiente manera ${greenColour}[${secuencia[@]}]${endColour}"
													elif [ $random_number -eq 0 ] && [ $numero_de_secuencias -eq 1 ]; then
														bet=$((${secuencia[0]} + ${secuencia[-1]} ))
														unset secuencia[0]
														unset secuencia[-1] 2>/dev/null
														secuencia=(${secuencia[@]})
														echo -e "${yellowColour}[+]${endColour}${grayColour} Invertimos${endColour} ${greenColour}$bet${endColour}"
														echo -e "${redColour}[!] Es impar, por tanto pierdes${endColour}"
														let money-=$bet
														if [ "$money" -lt 0 ]; then
															echo -e "\n${redColour}[+] Nos quedamos sin money, a chambear papa${endColour}"
															echo -e "${yellowColour}[+]${endColour} ${grayColour}El numero de jugadas totales, son${endColour} ${purpleColour}$jugadas_totales${endColour}\n"
															exit 0
														fi
														echo -e "${yellowColour}[+]${endColour}${grayColour} Tienes ${endColour} ${yellowColour}$money${endColour}"
														echo -e "${yellowColour}[+]${endColour}${grayColour} La secuencia se nos quedaria de la siguiente manera ${greenColour}[${secuencia[@]}]${endColour}"							

													fi
													#if terminado
													if [ "$check_number" -eq 0 ] && [ "$random_number" -ne 0 ]; then
														if [ "$numero_de_secuencias" -gt 1 ]; then
															bet=$((${secuencia[0]} + ${secuencia[-1]}))
															echo -e "${yellowColour}[+]${endColour}${grayColour} Invertimos${endColour} ${greenColour}$bet${endColour}"

															let money-=$bet
															echo -e "${yellowColour}[+]${endColour} Tienes${grayColour} ${endColour}${purpleColour}$money${endColour}"	
															#aqui hay un error porque estamos invirtiendo lo de antes
															if [ "$money" -lt 0 ]; then
																echo -e "\n${redColour}[+] Nos quedamos sin money, a chambear papa${endColour}"
																echo -e "${yellowColour}[+]${endColour} ${grayColour}El numero de jugadas totales, son${endColour} ${purpleColour}$jugadas_totales${endColour}\n"
																exit 0
															fi
															reward=$(($bet * 2))
															let money+=$reward
															
															echo -e "${yellowColour}[+]${endColour} ${grayColour}Es par, ganas${endColour}"
															
															echo -e "${yellowColour}[+]${endColour} ${grayColour}Por tanto ahora tienes:${endColour}${yellowColour} $money${endColour}"
															bet=$((${secuencia[0]} + ${secuencia[-1]}))
															secuencia+=($bet)
															secuencia=(${secuencia[@]})
															echo -e "${yellowColour}[+]${endColour}${grayColour} Nuestra nueva secuencia seria${endColour} ${greenColour}[${secuencia[@]}]${endColour}\n"
														#		si es igual a 1 entonces hay que hacer otra cosa que es sumar el unico valor
														elif [ "$numero_de_secuencias" -eq 1 ]; then
															secuencia=(${secuencia[@]})
															bet=${secuencia[0]}
															let money-=$bet
															if [ "$money" -lt 0 ]; then
																echo -e "\n${redColour}[+] Nos quedamos sin money, a chambear papa${endColour}"
																echo -e "${yellowColour}[+]${endColour} ${grayColour}El numero de jugadas totales, son${endColour} ${purpleColour}$jugadas_totales${endColour}\n"
																exit 0
															fi
															#aqui hay un error porque estamos invirtiendo lo de antes
															echo -e "${yellowColour}[+]${endColour}${grayColour} Invertimos${endColour} ${greenColour}$bet${endColour}"
															echo -e "${yellowColour}[+]${endColour}${grayColour} Tienes sera aqui el error ${endColour} ${yellowColour}$money${endColour}"

															secuencia+=($bet)
															secuencia=(${secuencia[@]})
															reward=$(($bet*2))
															let money+=$reward
															echo -e "${yellowColour}[+]${endColour} ${grayColour}Es par, ganas ${endColour}${yellowColour} $money${endColour}"
															echo -e "${yellowColour}[+]${endColour} Nuestra nueva secuencia seria de: ${greenColour}[${secuencia[@]}]${endColour}"
														#terminado este fi
														fi
												#fi terminado
													fi
													if [ "$check_number" -eq 1 ]; then

														if [ ! "$numero_de_secuencias" -eq 1 ]; then
															bet=$((${secuencia[0]} + ${secuencia[-1]} ))
															let money-=$bet
															if [ "$money" -lt 0 ]; then
																echo -e "\n${redColour}[+] Nos quedamos sin money, a chambear papa${endColour}"
																echo -e "${yellowColour}[+]${endColour} ${grayColour}El numero de jugadas totales, son${endColour} ${purpleColour}$jugadas_totales${endColour}\n"
																exit 0
															fi
															unset secuencia[0]
															unset secuencia[-1] 2>/dev/null
															secuencia=(${secuencia[@]})
															echo -e "${yellowColour}[+]${endColour}${grayColour} Invertimos${endColour} ${greenColour}$bet${endColour}"
															echo -e "${redColour}[!] Es impar, por tanto pierdes${endColour}"
															echo -e "${yellowColour}[+]${endColour}${grayColour} Tienes ${endColour} ${yellowColour}$money${endColour}"
															echo -e "${yellowColour}[+]${endColour}${grayColour} La secuencia se nos quedaria de la siguiente manera ${greenColour}[${secuencia[@]}]${endColour}"
														fi
														if [ "$numero_de_secuencias" -eq 1 ] && [ $random_number -ne 0 ]; then
															secuencia=(${secuencia[@]})
															bet=${secuencia[0]}
															money=$(($money - $bet))
															if [ "$money" -lt 0 ]; then
																echo -e "\n${redColour}[+] Nos quedamos sin money, a chambear papa${endColour}"
																	echo -e "${yellowColour}[+]${endColour} ${grayColour}El numero de jugadas totales, son${endColour} ${purpleColour}$jugadas_totales${endColour}\n"
																	exit 0
																fi
																#aqui hay un error porque estamos invirtiendo lo de antes
																echo -e "${yellowColour}[+]${endColour}${grayColour} Invertimos${endColour} ${greenColour}$bet${endColour}"
																echo -e "${yellowColour}[+]${endColour}${grayColour} Tienes ${endColour} ${yellowColour}$money${endColour}"
																secuencia+=($bet)
																secuencia=(${secuencia[@]})
																reward=$(($bet*2))
																let money+=$reward
																echo -e "${yellowColour}[+]${endColour} ${grayColour}Es par, ganas ${endColour}${yellowColour} $money${endColour}"
																echo -e "${yellowColour}[+]${endColour} Nuestra nueva secuencia seria de: ${greenColour}[${secuencia[@]}]${endColour}"
															fi
														fi
																							
													fi
											
																			
											#else que me servira para arreglar cuando sea conjunto vacio
											else
												echo -e "\n${redColour}[!] Hemos perdido nuestra secuencia${endColour}"
												secuencia=(${secuencia_renew[@]})
												echo -e "${yellowColour}[+]${endColour}${grayColour} Por tanto la secuencia se ha restablecido a la original, que seria esta:${endColour}${greenColour}${greenColour}[${secuencia[@]}]${endColour}"
					

											fi
										
												
										#	sleep 1	
										else
											echo -e "!aqui iria el por si elije impar"
										#fi termiando
										fi
									#else
									#	echo -e "es mayor a 50"
									#fi termiando
									#fi
									
									
									else
										echo -e "${yellowColour}[+]${endColour}${grayColour} Hemos llegado a por encima de${endColour} ${greenColour}$bet_to_renew${endColour}${grayColour}, por tanto la secuencia se renovara${endColour}"
										secuencia=(${secuencia_renew[@]})
										echo -e "${yellowColour}[+]${endColour}${grayColour} Se renovara a la siguiente secuencia${endColour}${greenColour} [${secuencia[@]}]${endColour}"
										bet_to_renew=$(($money+50))
										
										sleep 1
									
									fi
								else
									echo -e "\n${yellowColour}[+]${endColour}${grayColour} Hemos llegado a un minimo critico, se prosedera a reajustarlo${endColour}"
									secuencia=(${secuencia_renew[@]})
									bet_to_renew=$(($bet_to_renew-50))
									
									echo -e "${yellowColour}[+]${endColour} ${grayColour}El nuevo valor para el tope sera el de${endColour}${greenColour} $bet_to_renew${endColour}"
									echo -e "${yellowColour}[+]${endColour} ${grayColour}La nueva secuencia sera${endColour} ${greenColour}[${secuencia[@]}]${endColour}"
									sleep 1
								fi
						else
							echo -e "\n${redColour}[+] Nos quedamos sin money, a chambear papa${endColour}"
							echo -e "${yellowColour}[+]${endColour} ${grayColour}El numero de jugadas totales, son${endColour} ${purpleColour}$jugadas_totales${endColour}\n"

							exit 0
						fi
							tput cnorm
						
					done			
				#este else corresponde a si es que la apuesta es igual a la suma de la secuencia
				else
					echo -e "${redColour}[!] La suma de la secuencia debe ser igual a tu apuesta inicial${endColour}"
				fi
			#este else corresponde al initial_bet || ! initial -le 0
			else
				echo -e "\n${redColour}[!] Tu apuesta no puede ser mayor a tu dinero ni menor a 0${endColour}"
			fi
		#else que corresponde a ver siinitial ver es o no un numero
		else
			echo -e "\n${redColour}[!] Solo se permiten numeros${endColour}"
		fi
	else
		#este else corresponde a [ money -gt 0 ]
		echo -e "\n${redColour}[!] Tu dinero no puede ser 0${endColour}\n"
		helpPanel
	fi
}





while getopts "m:t:h" arg; do
	case "$arg" in
		m) money="$OPTARG"; regex='^[0-9]+(\.[0-9]+)?$';;
		t) tech="$OPTARG";;
		h) helpPanel
	esac
done

tech=${tech,,}

if [ $money ] && [ "$tech" ]; then
	if [[ $money =~ $regex ]]; then
		if [ $tech == "martingala" ]; then
			martingala
		elif [ "$tech" == "labouchereinverso" ] || [ "$tech" == "inverselabouchere" ]; then
			labouchereInverse
		else
			echo -e "\n${redColour}[!] Las tecnicas actuales solo son: Martingala y LabouchereInverso/InverseLabouchere${endColour}"
		fi
	fi
else
	helpPanel

fi
