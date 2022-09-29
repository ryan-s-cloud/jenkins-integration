pipeline {
    agent none
    parameters {
        choice(
            description: 'What scenario do you want to test?',
            choices: ['MassModification'],
            name: 'test_scenario'
        )
        choice(
            description: 'What type of test do you want to run?',
            choices: ['Peak_Load_Test', 'Endurance_Test'], // todo cleanup with groovy script string manip
            name: 'test_type'
        )
        choice(
            description: 'Which environment do you want to test?',
            choices: ['loadtest', 'qa'],
            name: 'test_environment'
        )
        string(
            description: 'How long in seconds should the test ramp?',
            defaultValue: '300',
            name: 'test_ramp_time'
        )
        string(
            description: 'How long should the test run in seconds?',
            defaultValue: '3600',
            name: 'test_execution_time'
        )
    }
    stages {
        stage( 'Execute Test' ) {
            agent {
                docker {
                    image 'justb4/jmeter'
                    // Run the container on the node specified at the
                    // top-level of the Pipeline, in the same workspace,
                    // rather than on a new node entirely:
                    reuseNode true
                }
            }            
            
            steps {
                script {
                    if( test_environment == 'loadtest' )
                    {
                        test_protocol = 'https'
                        test_base_url = 'loadtest.abc30release.com'
                    }
                    else
                    {
                        error "Invalid environment defined"
                    }
                }
                sh "rm setup/*.jtl || true"

                sh """\
                    --nongui \
                    --testfile "setup/MassModification.jmx" \
                    --logfile "setup/MassModification.jtl" \
                """.replaceAll( ' +', ' ' )

                /*
                sh "rm ${test_application.toLowerCase()}/*.jtl || true"

                sh """\
                    --nongui \
                    --testfile "setup/DataReset.jmx" \
                    --logfile "${test_application}/DataReset.jtl" \
                    --globalproperty ResetLoopCount=-1 \
                    --globalproperty ResetDataStartDelay=${test_ramp_time} \
                    --globalproperty ResetDataPause=${test_data_reset_pause} \
                    --globalproperty ResetDataDuration=${test_data_reset_duration}
                """.replaceAll( ' +', ' ' )
                sh """\
                    --nongui \
                    --runremote \
                    --testfile "${test_application.toLowerCase()}/${test_type}.jmx" \
                    --logfile "${test_application.toLowerCase()}/${test_type}.jtl" \
                    --globalproperty includecontroller.prefix=`pwd`/${test_application.toLowerCase()}/ \
                    --globalproperty Protocol=${test_protocol} \
                    --globalproperty BaseUrl=${test_base_url} \
                    --globalproperty RampUp=${test_ramp_time} \                                                                                     
                    --globalproperty LoadHoldDuration=${test_execution_time} \
                """.replaceAll( ' +', ' ' )

                // todo zip up jtl files, they are too big for slow internets
                archiveArtifacts "${test_application.toLowerCase()}/*.jtl"
                */
                archiveArtifacts 'setup/*.jtl'
            }
        }
    }
}
