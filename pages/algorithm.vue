<template>
  <v-container fluid class="pa-4">
    <v-row>
      <!-- Left side: Diagram container -->
      <v-col cols="8" class="diagram_container">
        <div id="myDiagramDiv" class="diagram"></div>
      </v-col>

      <!-- Right side: Data / Card content -->
      <v-col cols="4" class="pa-4 info-panel">
        <v-card
          class="py-5 pl-4"
          style="border: 1px solid white; border-radius: 30px"
        >
          <v-row>
            <v-col cols="3" class="mr-2"><b>Page ID</b></v-col>
            <v-col>{{ nodeData.key }}</v-col>
          </v-row>

          <v-row v-if="nodeData.hasOwnProperty('key')">
            <v-col cols="3" class="mr-2"><b>Module</b></v-col>
            <v-col>{{ findModule(nodeData.key) }}</v-col>
          </v-row>

          <v-row v-if="nodeData.hasOwnProperty('description')">
            <v-col cols="3" class="mr-2"><b>Title</b></v-col>
            <v-col>{{ nodeData.description }}</v-col>
          </v-row>

          <v-row v-if="nodeData.hasOwnProperty('video')">
            <v-col cols="3" class="mr-2"><b>Video</b></v-col>
            <v-col>{{ nodeData.video }}</v-col>
          </v-row>

          <v-row>
            <v-col cols="3" class="mr-2"><b>Target</b></v-col>
            <v-col>
              <div v-for="item in nodeData.user_types" :key="item">
                <v-chip v-if="item === 1" small color="blue">Non-User</v-chip>
                <v-chip v-if="item === 2" small color="orange">Low</v-chip>
                <v-chip v-if="item === 3" small color="orange">Moderate</v-chip>
                <v-chip v-if="item === 4" small color="red">Substantial</v-chip>
                <v-chip v-if="item === 5" small color="red">Severe</v-chip>
              </div>
            </v-col>
          </v-row>

          <v-row v-if="nodeData.hasOwnProperty('content')">
            <v-col cols="3" class="mr-2"><b>Display Content</b></v-col>
            <v-col>
              <div v-for="item in nodeData.content" :key="item" class="py-1">
                {{ item }}
              </div>
            </v-col>
          </v-row>

          <v-row v-if="nodeData.hasOwnProperty('input')">
            <v-col cols="3" class="mr-2"><b>Inputs</b></v-col>
            <v-col>{{ nodeData.input.input }}</v-col>
          </v-row>

          <v-row v-if="nodeData.hasOwnProperty('input')">
            <v-col cols="3" class="mr-2"><b>Input Variables</b></v-col>
            <v-col>{{ nodeData.input.data_name }}</v-col>
          </v-row>

          <v-row v-if="nodeData.hasOwnProperty('actions')">
            <v-col cols="3" class="mr-2"><b>Actions</b></v-col>
            <v-col>
              <div v-if="nodeData.actions.hasOwnProperty('next_page')">
                Next page rules ->
                <div
                  v-for="item in nodeData.actions.next_page"
                  :key="item"
                  class="py-1"
                >
                  {{ item }}
                </div>
              </div>
              <div v-if="nodeData.actions.hasOwnProperty('page_leave')">
                On page leave ->
                <div
                  v-for="item in nodeData.actions.page_leave"
                  :key="item"
                  class="py-1"
                >
                  {{ item }}
                </div>
              </div>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import go from 'gojs'
// import Algorithm from '~/components/lazyloads/Algorithm.vue'
export default {
  name: 'AlgorithmPage',
  layout: 'layout-admin',
  transition: 'fade',
  data() {
    return {
      nodeData: true,
      algoRouting: [
        { from: '', to: 'home', critical: false },
        {
          from: 'home',
          to: '1.0',
          critical: false,
        },
        { from: '1.0', to: '1.1', critical: false },
        { from: '1.1', to: '1.2', critical: false },
        { from: '1.1', to: '1.2', critical: false },
        { from: '1.1', to: '1.2A', critical: false },
        { from: '1.2', to: '1.3', critical: false },
        { from: '1.2A', to: '1.3A', critical: false },
        { from: '1.3A', to: '1.11', critical: false },
        { from: '1.3', to: '1.4', critical: false },
        { from: '1.4', to: '1.5', critical: false },
        { from: '1.5', to: '1.6', critical: false },
        { from: '1.6', to: '1.7', critical: false },
        { from: '1.7', to: '1.8', critical: false },
        { from: '1.8', to: '1.9', critical: false },
        { from: '1.9', to: '1.10', critical: false },
        { from: '1.9', to: '1.10C', critical: false },
        { from: '1.10', to: '1.11', critical: false },
        { from: '1.10C', to: '1.11C', critical: false },
        { from: '1.11C', to: '2C.0', critical: false },
        { from: '1.11', to: '1.12', critical: false },
        { from: '1.12', to: '1.13', critical: false },
        { from: '1.13', to: '1.14', critical: false },
        { from: '1.14', to: '1.15', critical: false },
        { from: '1.15', to: '2.0', critical: false },
        { from: '1.15', to: '2.0A', critical: false },
        { from: '2.0', to: '2.1', critical: false },
        { from: '2.1', to: '2.2', critical: false },
        { from: '2.2', to: '2.3', critical: false },
        { from: '2.0A', to: '2.3', critical: false },
        { from: '2.3', to: '2.4', critical: false },
        { from: '2.4', to: '2.5', critical: false },
        { from: '2.5', to: '2.6', critical: false },
        { from: '2.6', to: '2.7', critical: false },
        { from: '2.7', to: '2.8', critical: false },
        { from: '2.8', to: '2.9', critical: false },
        { from: '2.9', to: '3.0', critical: false },
        { from: '2.9', to: '4.0', critical: false },
        { from: '2.9', to: '5.0', critical: false },
        { from: '2.9', to: '4A.0', critical: false },
        { from: '2.9', to: '5A.0', critical: false },
        { from: '3.0', to: '3.1', critical: false },
        { from: '3.1', to: '3.2', critical: false },
        { from: '3.2', to: '3.3', critical: false },
        { from: '3.3', to: '3.4', critical: false },
        { from: '3.4', to: '3.5', critical: false },
        { from: '3.5', to: '3.6', critical: false },
        { from: '3.6', to: '3.7', critical: false },
        { from: '3.7', to: '3.8', critical: false },
        { from: '3.8', to: '4.0', critical: false },
        { from: '3.8', to: '4A.0', critical: false },
        { from: '3.8', to: '5.0', critical: false },
        { from: '3.8', to: '5A.0', critical: false },
        { from: '4.0', to: '4.1', critical: false },
        { from: '4.1', to: '4.2', critical: false },
        { from: '4.2', to: '4.3', critical: false },
        { from: '4.3', to: '4.4', critical: false },
        { from: '4.4', to: '4.5', critical: false },
        { from: '4.5', to: '4.6', critical: false },
        { from: '4.6', to: '4.7', critical: false },
        { from: '4.7', to: '4.8', critical: false },
        { from: '4.8', to: '4.9', critical: false },
        { from: '4.9', to: '5.0', critical: false },
        { from: '5.0', to: '5.1', critical: false },
        { from: '5.1', to: '5.2', critical: false },
        { from: '5.2', to: '5.3', critical: false },
        { from: '5.3', to: '5.4', critical: false },
        { from: '5.4', to: 'Review', critical: false },
        { from: '4A.0', to: '4A.1', critical: false },
        { from: '4A.1', to: '4A.2', critical: false },
        { from: '4A.2', to: '4A.3', critical: false },
        { from: '4A.3', to: '4A.4', critical: false },
        { from: '4A.4', to: '4A.5', critical: false },
        { from: '4A.5', to: '4A.6', critical: false },
        { from: '4A.6', to: '4A.7', critical: false },
        { from: '4A.7', to: '5A.0', critical: false },
        { from: '5A.0', to: '5A.1', critical: false },
        { from: '5A.1', to: '5A.2', critical: false },
        { from: '5A.2', to: '5A.3', critical: false },
        { from: '5A.3', to: '5A.4', critical: false },
        { from: '5A.4', to: 'Review', critical: false },
        { from: '2C.0', to: '2C.1', critical: false },
        { from: '2C.1', to: '2C.2', critical: false },
        { from: '2C.2', to: '2C.3', critical: false },
        { from: '2C.3', to: '2C.4', critical: false },
        { from: '2C.4', to: 'Review', critical: false },
      ],
      algoJSON: [
        {
          key: 'home',
          color: 'lightblue',
          decription: 'Introduction',
          user_types: [1, 2, 3, 4, 5],
          critical: false,
        },
        {
          key: '1.0',
          color: 'orange',
          description: 'Introduction',
          user_types: [1, 2, 3, 4, 5],
          colors: ['red', 'orange', 'gold', 'green', 'blue'],

          critical: false,
        },
        {
          key: '1.1',
          parent: 'home',
          color: 'orange',
          description: 'DAST Screener',
          user_types: [1, 2, 3, 4, 5],
          input: {
            description: 'First Dast Question',
            input:
              'Single multiple choice question seperating non users and users. ',
            data_name: 'dast_answers[0]',
          },
          actions: {
            next_page: ['IF answer = YES GOTO 1.2 ELSE GOTO 1.2A'],
          },
          critical: false,
        },
        {
          key: '1.2',
          color: 'orange',
          user_types: [2, 3, 4, 5],
          description: 'Drug Selection',
          input: {
            input: 'Multiple and single choice question on same page. ',
            data_name: 'selected_drugs, primary_drug',
          },
        },
        {
          key: '1.2A',
          color: 'orange',
          description: 'Quiz',
          user_types: [1],
          input: {
            description: 'Quiz Score',
            input:
              '10 questions, correct answers added together to calculate score out of 10.',
            data_name: 'quiz_score',
          },
        },
        {
          key: '1.3',
          color: 'orange',
          description: 'DAST Questionnaire',
          user_types: [2, 3, 4, 5],
          input: {
            input:
              '9 multiple choice questions, true answers added together to find DAST score. true answers recieve +1, aside from Q2 which recieves -1. (you are not able to stop using drugs). ',
            data_name: 'dast_score, user_type',
          },
          actions: {
            page_leave: [
              "IF dast_score = 0, SET user_type = 'Non-User'",
              "IF dast_score >=1 OR dast_score <=2, SET user_type = 'Low'",
              "IF dast_score >=13 OR dast_score <=5, SET user_type = 'Moderate'",
              "IF dast_score >=6 OR dast_score <=8, SET user_type = 'Substantial'",
              "IF dast_score > 8 SET user_type = 'Severe'",
            ],
          },
        },
        {
          key: '1.3A',
          color: 'orange',
          description: 'Quiz Feedback',
          user_types: [1],
          content: [
            'IF quiz_score = 0-3, DISPLAY quiz-feedback-0:',
            'IF quiz_score = 4-7 inclusive, DISPLAY quiz-feedback-1:',
            'IF quiz_score = >=8, DISPLAY quiz-feedback-2:',
          ],
        },
        {
          key: '1.4',
          description: 'Profile Feedback',
          color: 'orange',
          user_types: [2, 3, 4, 5],
          content: [
            'IF dast_score <= 2, DISPLAY dast-feedback-0',
            'IF dast_score >= 3 AND dast_score <= 5 DISPLAY dast-feedback-1',
            'IF dast_score >= 6, DISPLAY dast-feedback-2',
          ],
        },
        {
          key: '1.5',
          color: 'orange',
          description: 'Dast Feedback',
          user_types: [2, 3, 4, 5],
          content: [
            'IF dast-answer-0 = 0 DISPLAY dast-feedback-0',
            'IF dast-answer-1 = 0 DISPLAY dast-feedback-1',
            'IF dast-answer-2 = 1 DISPLAY dast-feedback-2',
            'IF dast-answer-3 = 0 DISPLAY dast-feedback-3',
            'IF dast-answer-4 = 0 DISPLAY dast-feedback-4',
            'IF dast-answer-5 = 0 DISPLAY dast-feedback-5',
            'IF dast-answer-6 = 0 DISPLAY dast-feedback-6',
            'IF dast-answer-7 = 0 DISPLAY dast-feedback-7',
            'IF dast-answer-8 = 0 DISPLAY dast-feedback-8',
          ],
        },
        {
          key: '1.6',
          color: 'orange',
          user_types: [2, 3, 4, 5],
          description: 'My Experiences',
          input: {
            input:
              'selection of 10 choices, 5 positives and 5 negatives. No max  on number of selected.',
            data_name: 'positivesList, negativesList',
          },
        },
        {
          key: '1.7',
          color: 'orange',
          description: 'My Experiences Feedback',
          user_types: [2, 3, 4, 5],
          content: [
            'IF no of selected positives > no of selected negatives, DISPLAY experiences-feedback-0',
            'IF no of selected positives < no of selected negatives = <6, DISPLAY experiences-feedback-1',
          ],
        },
        {
          key: '1.8',
          color: 'orange',
          description: 'Body Feedback',
          user_types: [2, 3, 4, 5],
          content: ['DISPLAY primary_drug AND user_type'],
        },
        {
          key: '1.9',
          color: 'orange',
          user_types: [2, 3, 4, 5],
          input: {
            description: 'Willingness to Change',
            input: '0-4 slider scale, Not at all, Slightly, Very, Extremely',
            data_name: 'willingness_to_change',
          },
          actions: {
            next_page: [
              'IF dast_score <= 5 GOTO 1-10',
              'IF dast_score >= 6 GOTO 1-10C',
            ],
          },
        },
        {
          key: '1.10',
          color: 'orange',
          user_types: [2, 3],
          description: 'Reducing Risk',
        },
        {
          key: '1.11',
          color: 'orange',
          user_types: [2, 3],
          input: {
            description: 'Personal Goals',
            input:
              '3 Open Text input boxes - user is required to enter value for at least 1.',
            data_name: 'personal_goals',
          },
        },
        {
          key: '1.10C',
          color: 'orange',
          user_types: [3],
          input: {
            description: 'Confidence to Change',
            input:
              '0-10 scale, 11-point scale 0 = not at all confident, 10 = extremely confident',
            data_name: 'confidence_score',
          },
        },
        {
          key: '1.11C',
          color: 'orange',
          user_types: [4, 5],
          actions: {
            next_page: ['GOTO 2C-0'],
          },
          content: [
            'IF confidence_score => 6, DISPLAY confidence-feedback-0',
            'IF confidence_score <6, DISPLAY confidence-feedback-1',
          ],
        },
        {
          key: '1.12',
          color: 'orange',
          user_types: [2, 3],
          description: 'Your Peers',
          input: {
            description: 'Descriptive Norms',
            input:
              'Two sliders/values, 0 - 100 range, default to 0. User is required to answer two questions, answers 68 and 34 respectively. The user will move the slider to answer. ',
            data_name: 'descriptive_norms',
          },
        },
        {
          key: '1.13',
          color: 'orange',
          user_types: [2, 3],
          description: 'Your Peers 2',
          content: [
            'IF descriptive-norms-0 <= (68 - errorMargin) || descriptive-norms-0 >= (32 + errorMargin) DISPLAY descriptive-norms-fb-0 (correct) ELSE DISPLAY descriptive-norms-fb-0 (incorrect)',
            'IF descriptive-norms-1 <= (68 - errorMargin) || descriptive-norms-0 >= (32 + errorMargin) DISPLAY descriptive-norms-fb-1 (correct) ELSE DISPLAY descriptive-norms-fb-1 (incorrect)',
            'Note: error margin is 5%',
          ],
        },
        {
          key: '1.14',
          color: 'orange',
          user_types: [2, 3],
          description: 'Friends and Family 1',
          input: {
            description: 'Injunctive Norms',
            input:
              "Three multiple choice questions with answers approve, disapprove, and wouldn't care.",
            data_name: 'injunctive_norms',
          },
        },
        {
          key: '1.15',
          color: 'orange',
          user_types: [2, 3],
          description: 'Friends and Family 2',
          actions: {
            next_page: ['IF user type is Non-User GOTO 2-A0', 'ELSE go to 2-0'],
          },
          content: [
            "IF injunctive-norm-0 = approve, disapprove, wouldn't care DISPLAY injunctive-answer-{0-2}",
            "IF injunctive-norm-1 = approve, disapprove, wouldn't care DISPLAY injunctive-answer-{3-5}",
            "IF injunctive-norm-2 = approve, disapprove, wouldn't care DISPLAY injunctive-answer-{6-8}",
            'IF injunctive-norm-0 = approve AND injunctive-norm-1 = disapprove AND injunctive-norm-2 = disapprove DISPLAY injunctive-overall-0',
            'IF injunctive-norm-0 = disapprove AND injunctive-norm-1 = approve AND injunctive-norm-2 = approve DISPLAY injunctive-overall-2',
            "IF injunctive-norm-0 = wouldn't care OR injunctive-norm-1 = wouldn't care OR injunctive-norm-2 = wouldn't care DISPLAY injunctive-overall-2",
          ],
        },
        {
          key: '2C.0',
          color: 'blue',
          user_types: [4, 5],
          description: 'Welcome to My Supports',
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/M2C+Intro.mp4',
        },
        {
          key: '2C.1',
          color: 'blue',
          user_types: [4, 5],
          description: 'Choose Supports',
          input: {
            description: 'Choose your supports (stored as the users needs)',
            input: 'select up to 10 items from the options available',
            data_name: 'needs',
          },
        },
        {
          key: '2C.2',
          color: 'blue',
          user_types: [4, 5],
          description: 'Your Supports',
          content: [
            'FOR all selected (needs-x) DISPLAY feedback (needs-feedback-x)',
          ],
        },
        {
          key: '2C.3',
          color: 'blue',
          user_types: [4, 5],
          description: 'Choose Advice',
          input: {
            description: 'Choose helpful options to reduce intake.',
            input: 'Select up to 6 items from the options available',
            data_name: 'helps',
          },
        },
        {
          key: '2C.4',
          color: 'blue',
          user_types: [4, 5],
          description: 'Your Advice',
          content: [
            'FOR all selected  (needs-x) DISPLAY feedback (needs-feedback-x)',
          ],
        },
        {
          key: '2.0',
          color: 'blue',
          user_types: [2, 3],
          description: 'Welcome to My Relationships',
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/M2A+Intro.mp4',
        },
        {
          key: '2.1',
          color: 'blue',
          user_types: [2, 3],
          description: 'Impacts of Drug Use On Relionships',
          input: {
            description:
              'All things considered, indicate the **negative** impact of your drug use on your..',
            input: 'Three 0-10 scale sliders (Friends, Family, Other)',
            data_name: 'relationships_scores (array)',
          },
        },
        {
          key: '2.2',
          color: 'blue',
          user_types: [2, 3],
          description: 'Relationships Feedback',
          content: [
            'IF relationships_scores[0] > 5 DISPLAY relationships-feedback-0 ELSE relationships-feedback-1',
            'IF relationships_scores[1] > 5 DISPLAY relationships-feedback-2 ELSE relationships-feedback-3',
            'IF relationships_scores[2] > 5 DISPLAY relationships-feedback-3 ELSE relationships-feedback-4',
            'IF total( relationships_scores) > 5 DISPLAY relationships-feedback-6 (How to repair a relationship advicse)',
          ],
        },
        {
          key: '2.0A',
          color: 'blue',
          user_types: [1],
          description: 'Welcome to My Relationships (Non-User intro)',
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/M2A+Intro.mp4',
        },
        {
          key: '2.3',
          color: 'blue',
          user_types: [1, 2, 3],
          description: 'Your Priorities',
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/M2A+Intro.mp4',
          input: {
            description: 'Quality of Life Score, Life Progress Score',
            input:
              'Two sliders/values, 0 - 10 range, default to 5. life_values_score is calculated by adding both values. ',
            data_name: 'life_values_score',
          },
        },
        {
          key: '2.4',
          color: 'blue',
          user_types: [1, 2, 3],
          description: 'Priorities Feedback',
          content: [
            'IF life_values_score >= 10 DISPLAY life-values-feedback-0 ELSE DISPLAY life-values-feedback-1',
          ],
        },
        {
          key: '2.5',
          color: 'blue',
          user_types: [1, 2, 3],
          description: 'Your Awareness',
          input: {
            description:
              "It seems that I am 'running on automatic' without much awareness of what I am doing. (Agree)",
            input:
              '5, 1-7 range scale sliders (Friends, Family, Other). variable is  an array of scores.',
            data_name: 'relationships_scores',
          },
        },
        {
          key: '2.6',
          color: 'blue',
          user_types: [1, 2, 3],
          description: 'Your Awareness Feedback',
          content: [
            'IF relationships_scores > 17 DISPLAY relationship-feedback-0',
            'ELSE DISPLAY relationship-feedback-1',
          ],
        },

        {
          key: '2.7',
          color: 'blue',
          user_types: [1, 2, 3],
          description: 'Your Decision Making',
          input: {
            description:
              'How true is this statement... (Never True,Very Seldom True,Seldom True,Sometimes True, Frequently True, Almost Always True,Always True)',
            input: '9, 1-7 range scale sliders (addition of all scores)',
            data_name: 'decisions_score ',
          },
        },
        {
          key: '2.8',
          color: 'blue',
          user_types: [1, 2, 3],
          description: 'Your Decision Making Feedback',
          content: [
            'IF decisions_score > 32.8 DISPLAY decisions-feedback-1',
            'ELSE DISPLAY decisions-feedback-0',
          ],
        },
        {
          key: '2.9',
          color: 'blue',
          user_types: [1, 2, 3],
          description: 'Fully Present Metaphor',
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/Activity-4.mp4',
          actions: {
            next_page: [
              'IF lowLifeValuesScore = < 10, GOTO 3-0 ELSE UNLOCK 3-0',
              'IF usertype IS druguser',
              '    awareness_score = >17 AND decision_score= >32.8,  GOTO 5-0 AND UNLOCK 4-0',
              '    awareness_score = <17 OR decision_score = <32.8, GOTO 4-0',
              'ELSE',
              '    awareness_score = >17 AND decision_score = >32.8, GOTO Review AND UNLOCK 4A-0',
              '    awareness_score = <17 OR decision_score = <32.8, GOTO 4A-0',
            ],
          },
        },
        {
          key: '3.0',
          color: 'green',
          user_types: [1, 2, 3],
          description: 'Welcome to My Values',
          video: 'https://myuse-videos.s3.eu-west-1.amazonaws.com/M3+Intro.mp4',
        },
        {
          key: '3.1',
          color: 'green',
          user_types: [1, 2, 3],
          description: 'Defining Values',
          content: ['RECALL personal_values'],
        },
        {
          key: '3.2',
          color: 'green',
          user_types: [1, 2, 3],
          description: 'Select Values',
          input: {
            description: 'Select up to three values that are important to you.',
            input: 'Selection of 10 options. Max 3.',
            data_name: 'decisions_score ',
          },
        },
        {
          key: '3.3',
          color: 'green',
          user_types: [1, 2, 3],
          description: 'Rate Your Values',
          input: {
            description: 'Rate each of your selected values 0-9.',
            input: '0-10. Defaults to 0.',
            data_name: 'values_score ',
          },
        },
        {
          key: '3.4',
          color: 'green',
          user_types: [1, 2, 3],
          description: 'Rate Your Values',
          content: [
            'FOR all selected  (values-score-x) DISPLAY feedback (values-feedback-x)',
          ],
        },
        {
          key: '3.5',
          color: 'green',
          user_types: [1, 2, 3],
          description: 'MyUSE SMART Goals',
          input: {
            description: 'Input you MyUSE SMART goal selections',
            input: '5 inputs for MyUSE SMART (and B for Barriers)',
            data_name: 'smart_goals ',
          },
        },
        {
          key: '3.6',
          color: 'green',
          user_types: [1, 2, 3],
          description: 'Set your MyUSE SMART Goals',
          input: {
            description: 'Input your SMART goal selections',
            input: '5 inputs for SMART (and B for Barriers)',
            data_name: 'smart_goals ',
          },
        },
        {
          key: '3.7',
          color: 'green',
          user_types: [1, 2, 3],
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/Activity+3.8.mp4',
          description: 'Your SMART Goal',
          input: {
            description: 'How willing are you to pursue your values now?',
            input: '1 slider, values 0-10',
            data_name: 'willingness',
          },
        },
        {
          key: '3.8',
          color: 'green',
          user_types: [1, 2, 3],
          description: 'Reflection',
          content: ['FOR each smart_goal DISPLAY FEEDBACK'],
          actions: {
            next_page: [
              'IF awareness_score = >17 OR decision_score= >32.8',
              '    IF low, moderate GOTO 4-0',
              '    ELSE GOTO 4A-0',
              'ELSE',
              '    IF low, moderate GOTO 5-0',
              '    ELSE GOTO 5A-0',
              '    AND',
              '     IF low, moderate UNLOCK 4-0',
              '    ELSE  UNLOCK 4A-0',
            ],
          },
        },
        {
          key: '4.0',
          color: 'grey',
          user_types: [2, 3],
          description: 'Welcome to My Mindful Decisions',
          video: 'https://myuse-videos.s3.eu-west-1.amazonaws.com/M4+Intro.mp4',
        },
        {
          key: '4.1',
          color: 'grey',
          user_types: [2, 3],
          description: 'Decision Making',
        },
        {
          key: '4.2',
          color: 'grey',
          user_types: [2, 3],
          description: 'Reasons For Use',
          input: {
            description: 'What are the main reasons you use drugs? ',
            input: 'Select from 8 options.Max 3.',
            data_name: 'reasons_for_use',
          },
        },
        {
          key: '4.3',
          color: 'grey',
          user_types: [2, 3],
          description: 'Your Reasons',
          content: [
            'FOR all selected (reasons-x) DISPLAY feedback (reasons-feedback-x)',
          ],
        },
        {
          key: '4.4',
          color: 'grey',
          user_types: [2, 3],
          description: ' Influences',
        },
        {
          key: '4.5',
          color: 'grey',
          user_types: [2, 3],
          description: 'Darraghs Story',
          input: {
            description: 'What are Darragh’s influences in this scenario?',
            input: 'Choose up to 6 influences',
            data_name: 'hooks',
          },
        },
        {
          key: '4.6',
          color: 'grey',
          user_types: [2, 3],
          description: 'Your Influences',
          input: {
            description:
              'Can you think of any other influences you might have?',
            input: 'Open text field for other influences',
            data_name: 'influences',
          },
          content: ['DISPLAY all selected (hooks-x)'],
        },
        {
          key: '4.7',
          color: 'grey',
          user_types: [2, 3],
          description: 'Mindful Decisions',
          content: [
            'DISPLAY all selected (hooks-x)',
            'DISPLAY all selected (reasons-for-use-x)',
          ],
        },
        {
          key: '4.8',
          color: 'grey',
          user_types: [2, 3],
          description: 'Mindful Decisions',
          content: [
            'DISPLAY all personal_values (hooks-x)',
            'DISPLAY all negatives (reasons-for-use-x)',
          ],
        },
        {
          key: '4.9',
          color: 'grey',
          user_types: [2, 3],
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/Activity-9.mp4',
          description: 'Mindful Decisions',
          input: {
            description:
              'Would you consider using the mindful decision process for yourself?',
            input: 'Button',
            data_name: 'Not Stored!',
          },
        },
        {
          key: '4A.0',
          color: 'orange',
          user_types: [1],
          description: 'Welcome to My Surroundings',
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/Intro+M4A.mp4',
        },
        {
          key: '4A.1',
          color: 'orange',
          user_types: [1],
          description: 'Decision Making',
        },
        {
          key: '4A.2',
          color: 'orange',
          user_types: [1],
          input: {
            description:
              'What could influence your or someone else’s decision to use drugs?',
            input: 'Button',
            data_name: 'Not Stored!',
          },
        },
        {
          key: '4A.3',
          color: 'orange',
          user_types: [1],
          description: 'Influences Feedback',
          content: ['DISPLAY all FEEDBACK for (influences-x)'],
        },
        {
          key: '4A.4',
          color: 'orange',
          user_types: [1],
          description: 'Influences Feedback',
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/Influence+and+Effect+M4.mp4',
          content: ['DISPLAY all influences'],
        },
        {
          key: '4A.5',
          color: 'orange',
          user_types: [1],
          description: 'Darraghs Story',
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/Activity-8.mp4',
        },
        {
          key: '4A.6',
          color: 'orange',
          user_types: [1],
          description: 'Darraghs Story',
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/Activity-9.mp4',
        },
        {
          key: '4A.7',
          color: 'orange',
          user_types: [1],
          description: 'Mindful Decisions',
        },
        {
          key: '5.0',
          color: 'pink',
          user_types: [2, 3],
          description: 'Welcome to My Plan',
          video: 'https://myuse-videos.s3.eu-west-1.amazonaws.com/Intro+M5.mp4',
        },
        {
          key: '5.1',
          color: 'pink',
          user_types: [2, 3],
          description: 'Reducing Harm',
          input: {
            description:
              'What harm reduction practices would you consider using? Choose 3.',
            input: '10 Options, choose a max of 3.',
            data_name: 'hr_practices',
          },
        },
        {
          key: '5.2',
          color: 'pink',
          user_types: [2, 3],
          description: 'Harm Reduction Practices',
          content: [
            'FOR all (hr_practices-x) DISPLAY feedback (hr_practices-feedback-x)',
            'IF hr_practices-7 DISPLAY injunctive-norms',
            'IF hr_practices-8 DISPLAY reasons_for_use',
          ],
        },
        {
          key: '5.3',
          color: 'pink',
          user_types: [2, 3],
          description: 'Commitment to Your Plan',
          content: ['DISPLAY feedback (hr_practices-x)'],
          input: {
            description: 'How committed are you to implementing this plan?',
            input: '0-4 slider.',
            data_name: 'hr_practices',
          },
        },
        {
          key: '5.4',
          color: 'pink',
          user_types: [2, 3],
          description: 'Your Plan',
        },

        {
          key: '5A.0',
          color: 'yellow',
          user_types: [1],
          description: 'Welcome to Maintaining My Values',
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/M5A+Intro.mp4',
        },
        {
          key: '5A.1',
          color: 'yellow',
          user_types: [1],
          video:
            'https://myuse-videos.s3.eu-west-1.amazonaws.com/POTB+Activity.mp4',
          description: 'Bus Analogy',
          content: [
            'IF behavioural_awareness_score > 17 DISPLAY act-fb-0',
            'ELSE DISPLAY act-fb-1',
          ],
        },
        {
          key: '5A.2',
          color: 'yellow',
          user_types: [1],
          content: [
            'DISPLAY personal_values',
            'DISPLAY values',
            'DISPLAY influences',
          ],
        },
        {
          key: '5A.3',
          color: 'yellow',
          user_types: [1],
          description: 'Decision Making Skills',
          input: {
            description:
              'Which skills would you consider using? Select your top 4.',
            input: '0-7 slider. Max 4.',
            data_name: 'skills',
          },
        },
        {
          key: '5A.4',
          color: 'yellow',
          user_types: [1],
          description: 'Decision Making Skills',

          content: [
            'DISPLAY skills',
            'IF skills-7 DISPLAY injunctive-norms',
            'IF skills-8 DISPLAY reasons_for_use',
          ],
        },
        {
          key: 'Review',
          color: 'blue',
          user_types: [1, 2, 3, 4, 5],
          description: 'All Feedback',
          content: [],
        },
      ],
    }
  },
  mounted() {
    const self = this
    this.nodeData = this.algoJSON[1]
    const $ = go.GraphObject.make
    const myDiagram = $(go.Diagram, 'myDiagramDiv', {
      'undoManager.isEnabled': true,
      initialAutoScale: go.Diagram.UniformToFill,
      layout: $(go.TreeLayout, { comparer: go.LayoutVertex.smartComparer }),
    })

    // Define a simple Node template
    myDiagram.nodeTemplate = $(
      go.Node,
      'Auto',
      $(
        go.Shape,
        'RoundedRectangle',
        { strokeWidth: 0, fill: 'white' },
        new go.Binding('fill', 'color')
      ),
      $(
        go.Panel,
        'Table',
        { defaultAlignment: go.Spot.Left, margin: 4 },
        $(go.RowColumnDefinition, { column: 1, width: 4 }),
        $(
          go.TextBlock,
          { row: 0, column: 0, columnSpan: 3, alignment: go.Spot.Center },
          { font: 'bold 12pt sans-serif' },
          new go.Binding('text', 'key')
        ),
        $(go.TextBlock, { row: 2, column: 2 }, new go.Binding('text', 'prop2'))
      ),
      {
        // Tooltip for each node
        toolTip: $(
          go.Adornment,
          'Spot',
          { background: 'transparent' },
          $(go.Placeholder, { padding: 5 }),
          $(
            go.TextBlock,
            {
              alignment: go.Spot.Top,
              alignmentFocus: go.Spot.Bottom,
              stroke: 'red',
            },
            new go.Binding('text', 'key', s => 'key: ' + s)
          ),
          $(
            go.TextBlock,
            'Bottom',
            {
              alignment: go.Spot.Bottom,
              alignmentFocus: go.Spot.Top,
              stroke: 'red',
            },
            new go.Binding('text', 'color', s => 'color: ' + s)
          )
        )
      }
    )

    // Set the diagram model
    myDiagram.model = new go.GraphLinksModel(this.algoJSON, this.algoRouting)

    // Define the Link template (removed unused parameter)
    myDiagram.linkTemplate = $(
      go.Link,
      $(
        go.Shape,
        { strokeWidth: 2 },
        new go.Binding('stroke', 'critical', () => 'lightblue')
      )
    )

    // Update nodeData on node click
    myDiagram.addDiagramListener('ObjectSingleClicked', function (e) {
      const part = e.subject.part
      if (!(part instanceof go.Link)) {
        self.nodeData = part.data
      }
    })

    myDiagram.layout = new go.TreeLayout({ angle: 90, layerSpacing: 65 })
  },
  methods: {
    findModule(module_String) {
      return module_String.includes('.') ? module_String.split('.')[0] : module_String
    },
  },
}
</script>

<style scoped>
.diagram_container {
  /* Make the diagram container tall enough for easier viewing */
  min-height: 80vh;
  display: flex;
  flex-direction: column;
}

.diagram {
  flex: 1; /* fill remaining vertical space */
  width: 100%;
  overflow: hidden;
  cursor: move;
}
</style>